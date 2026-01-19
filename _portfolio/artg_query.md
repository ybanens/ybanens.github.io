---
title: "Querying the ARTG"
excerpt: "The Therapeutic Goods Administration (TGA) provides a [searchable view](https://compliance.health.gov.au/artg/) of the Australian Register of Therapeutic Goods (ARTG). I used it to identify the number of pieces of complex medical equipment approved for supply in Australia."
collection: portfolio
---

## Querying the ARTG

The Therapeutic Goods Administration (TGA) provides a [searchable view](https://compliance.health.gov.au/artg/) of the Australian Register of Therapeutic Goods (ARTG). It provides information on parmaceutical goods and medical devices, along with other smaller categories. Each entry includes information on the good's Global Medical Device Nomenclature (GMDN) number and description. 

I sought to identify which medical devices constituted complex or capital medical equipment (see [this definition of 'medical equipment']([digitaledition.24x7mag.com/x7mag/diged/201906/index.html](https://digitaledition.24x7mag.com/x7mag/diged/201906/index.html)) by Wang). It was necessary to use GMDN descriptions, since there is no categorical way to identify them. Since there are over 7000 distinct GMDN codes in the ARTG, I used [Claude AI](https://claude.ai/) to speed the process of identification. 

I submitted the following query to Claude and it generated the Python code below. 

### The query

> “These are the GMDN codes for all approved medical devices in Australia. I want to work out which ones are complex or capital medical equipment, on the basis that it will be more relevant to right to repair legislation. it has been suggested that 'medical equipment' be defined as a medical device which is: (i) durable and reusable for multiple patients for at least three years; (ii) serialized (i.e. assigned a serial number), and; (iii) has unit acquisition cost over US$2500. Based on this definition, look through these codes and generate a CSV file listing the codes that are likely to meet the definition.”

### The output

```
#!/usr/bin/env python3
"""
Analyse GMDN codes to identify complex/capital medical equipment.

Criteria for complex medical equipment:
1. Durable and reusable for multiple patients for at least three years
2. Serialized (assigned a serial number)
3. Unit acquisition cost over US$2500
"""

import csv
import re

def load_gmdn_codes(filepath):
    """Load GMDN codes from CSV file."""
    codes = []
    with open(filepath, 'r', encoding='utf-8-sig') as f:
        reader = csv.reader(f)
        for row in reader:
            if row:
                codes.append(row[0].strip())
    return codes

def extract_code_and_description(line):
    """Extract GMDN code number and description."""
    # Format: "CODE - Description" or CODE - Description
    line = line.strip('"')
    if ' - ' in line:
        parts = line.split(' - ', 1)
        code = parts[0].strip()
        description = parts[1].strip() if len(parts) > 1 else ''
        return code, description
    return '', line

def is_likely_complex_equipment(code, description):
    """
    Determine if a GMDN code likely represents complex medical equipment.
    
    Returns: (bool, str) - (is_complex, reason)
    """
    desc_lower = description.lower()
    
    # Definite exclusions - single-use, disposable, or low-cost items
    exclusion_patterns = [
        'single-use', 'single use', 'disposable', 'sterile barrier',
        'bandage', 'dressing', 'gauze', 'tape', 'adhesive strip',
        'glove', 'condom', 'pad', 'sponge', 'swab', 'wipe',
        'needle', 'syringe', 'lancet', 'blade', 'scalpel',
        'suture', 'staple', 'clip', 'pin',
        'catheter', 'tube', 'drain', 'bag', 'container', 'bottle',
        'cover', 'drape', 'sheet', 'blanket', 'gown', 'mask',
        'filter', 'membrane', 'patch',
        'kit', 'set', 'pack', 'tray',
        'brush', 'comb', 'applicator', 'stick', 'swab',
        'reagent', 'solution', 'medium', 'gel', 'powder',
        'implant', 'prosthesis', 'graft', 'stent',
        'bur,', 'file,', 'rongeur', 'forceps', 'scissors', 'clamp',
        'retractor', 'speculum', 'dilator', 'probe',
        'accessory', 'attachment', 'component', 'part'
    ]
    
    for pattern in exclusion_patterns:
        if pattern in desc_lower:
            return False, f"Excluded: {pattern}"
    
    # Definite inclusions - high-value capital equipment
    # These are typically durable, serialized, and expensive
    inclusion_patterns = [
        # Imaging equipment
        ('x-ray', 'imaging'),
        ('ct ', 'imaging'),
        ('computed tomography', 'imaging'),
        ('mri', 'imaging'),
        ('magnetic resonance', 'imaging'),
        ('ultrasound system', 'imaging'),
        ('ultrasonic system', 'imaging'),
        ('fluoroscopy', 'imaging'),
        ('mammography', 'imaging'),
        ('pet/', 'imaging'),
        ('spect', 'imaging'),
        ('gamma camera', 'imaging'),
        ('image intensifier', 'imaging'),
        ('radiographic', 'imaging'),
        ('radiotherapy', 'therapy'),
        ('radiation therapy', 'therapy'),
        ('linear accelerator', 'therapy'),
        ('brachytherapy', 'therapy'),
        
        # Surgical systems
        ('surgical system', 'surgical'),
        ('surgical robot', 'surgical'),
        ('phacoemulsification system', 'surgical'),
        ('surgical laser', 'surgical'),
        ('electrosurgical unit', 'surgical'),
        ('surgical drill', 'surgical'),
        ('surgical saw', 'surgical'),
        ('surgical microscope', 'surgical'),
        ('operative microscope', 'surgical'),
        ('endoscopy system', 'surgical'),
        ('laparoscopy system', 'surgical'),
        ('arthroscopy system', 'surgical'),
        
        # Anaesthesia and respiratory
        ('anaesthesia machine', 'anaesthesia'),
        ('anaesthesia workstation', 'anaesthesia'),
        ('anaesthetic system', 'anaesthesia'),
        ('ventilator', 'respiratory'),
        ('respirator', 'respiratory'),
        ('cpap', 'respiratory'),
        ('bipap', 'respiratory'),
        
        # Monitoring and diagnostics
        ('patient monitor', 'monitoring'),
        ('vital signs monitor', 'monitoring'),
        ('ecg machine', 'monitoring'),
        ('electrocardiograph', 'monitoring'),
        ('eeg', 'monitoring'),
        ('electroencephalograph', 'monitoring'),
        ('defibrillator', 'emergency'),
        ('cardioverter', 'emergency'),
        ('infusion pump', 'infusion'),
        ('infusion system', 'infusion'),
        ('syringe pump', 'infusion'),
        ('dialysis', 'therapy'),
        
        # Laboratory
        ('analyser', 'laboratory'),
        ('analyzer', 'laboratory'),
        ('blood gas', 'laboratory'),
        ('haematology', 'laboratory'),
        ('chemistry analyser', 'laboratory'),
        ('immunoassay', 'laboratory'),
        ('spectrophotometer', 'laboratory'),
        ('microscope', 'laboratory'),
        ('centrifuge', 'laboratory'),
        ('incubator', 'laboratory'),
        ('sterilizer', 'sterilization'),
        ('autoclave', 'sterilization'),
        
        # Beds and patient handling
        ('bed, electric', 'patient care'),
        ('bed, adjustable', 'patient care'),
        ('bed, intensive care', 'patient care'),
        ('bed, operating', 'patient care'),
        ('examination table, electric', 'patient care'),
        ('operating table', 'patient care'),
        ('patient lift', 'patient care'),
        
        # Therapy equipment
        ('physiotherapy', 'therapy'),
        ('rehabilitation', 'therapy'),
        ('hyperbaric', 'therapy'),
        ('therapeutic system', 'therapy'),
        ('therapy system', 'therapy'),
        
        # Dental equipment (major)
        ('dental unit', 'dental'),
        ('dental chair', 'dental'),
        ('dental x-ray', 'dental'),
        ('dental laser', 'dental'),
        
        # Other capital equipment
        ('surgical table', 'surgical'),
        ('operative table', 'surgical'),
        ('surgical light', 'surgical'),
        ('operative light', 'surgical'),
        ('warming system', 'patient care'),
        ('cooling system', 'patient care'),
        ('pressure washer', 'sterilization'),
        ('washer-disinfector', 'sterilization'),
    ]
    
    for pattern, category in inclusion_patterns:
        if pattern in desc_lower:
            return True, f"Included: {category} - {pattern}"
    
    # Medium-probability inclusions - likely serialized and durable
    # but need more careful consideration
    medium_patterns = [
        ('system', 'system-based equipment'),
        ('unit', 'unit-based equipment'),
        ('machine', 'machine equipment'),
        ('equipment', 'general equipment'),
        ('device, powered', 'powered device'),
        ('workstation', 'workstation'),
        ('console', 'console'),
        ('generator', 'generator'),
        ('pump, ', 'pump equipment'),
        ('monitor', 'monitoring equipment'),
        ('recorder', 'recording equipment'),
        ('stimulator', 'stimulation equipment'),
        ('insufflator', 'surgical equipment'),
        ('coagulator', 'surgical equipment'),
        ('cryosurgical', 'surgical equipment'),
        ('electrosurgical', 'surgical equipment'),
        ('therapeutic', 'therapy equipment'),
    ]
    
    # Check for medium patterns but exclude if it has exclusion indicators
    simple_indicators = ['manual', 'hand-held', 'handheld', 'portable', 'reusable', 'non-powered']
    has_simple_indicator = any(indicator in desc_lower for indicator in simple_indicators)
    
    if not has_simple_indicator:
        for pattern, category in medium_patterns:
            if pattern in desc_lower:
                return True, f"Medium probability: {category}"
    
    return False, "Not identified as complex equipment"

def main():
    print("Analysing GMDN codes for complex medical equipment...")
    
    # Load data
    codes = load_gmdn_codes('/mnt/user-data/uploads/GMDN_Only_16-Dec-2025.csv')
    print(f"Total GMDN codes loaded: {len(codes)}")
    
    # Analyse codes
    complex_equipment = []
    excluded = []
    
    for line in codes:
        code, description = extract_code_and_description(line)
        if code and description:
            is_complex, reason = is_likely_complex_equipment(code, description)
            
            if is_complex:
                complex_equipment.append({
                    'code': code,
                    'description': description,
                    'reason': reason
                })
            else:
                excluded.append({
                    'code': code,
                    'description': description,
                    'reason': reason
                })
    
    # Output results
    print(f"\nComplex equipment identified: {len(complex_equipment)}")
    print(f"Excluded: {len(excluded)}")
    
    # Save to CSV
    output_file = '/mnt/user-data/outputs/complex_medical_equipment_gmdn.csv'
    with open(output_file, 'w', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow(['GMDN_Code', 'Description', 'Classification_Reason'])
        
        for item in sorted(complex_equipment, key=lambda x: (x['code'].isdigit(), int(x['code']) if x['code'].isdigit() else 0, x['code'])):
            writer.writerow([item['code'], item['description'], item['reason']])
    
    print(f"\nResults saved to: {output_file}")
    
    # Save summary statistics
    summary_file = '/mnt/user-data/outputs/analysis_summary.txt'
    with open(summary_file, 'w', encoding='utf-8') as f:
        f.write("GMDN Code Analysis Summary\n")
        f.write("=" * 50 + "\n\n")
        f.write(f"Total GMDN codes analysed: {len(codes)}\n")
        f.write(f"Complex equipment identified: {len(complex_equipment)}\n")
        f.write(f"Excluded: {len(excluded)}\n")
        f.write(f"Percentage identified as complex: {len(complex_equipment)/len(codes)*100:.1f}%\n\n")
        
        # Category breakdown
        f.write("\nCategory Breakdown:\n")
        f.write("-" * 50 + "\n")
        categories = {}
        for item in complex_equipment:
            reason = item['reason']
            # Extract category
            if ': ' in reason:
                category = reason.split(': ')[1].split(' - ')[0]
            else:
                category = reason.split(': ')[0] if ': ' in reason else 'Other'
            
            categories[category] = categories.get(category, 0) + 1
        
        for category, count in sorted(categories.items(), key=lambda x: x[1], reverse=True):
            f.write(f"{category}: {count}\n")
        
        f.write("\n\nSample Equipment (first 20):\n")
        f.write("-" * 50 + "\n")
        for item in complex_equipment[:20]:
            f.write(f"{item['code']}: {item['description']}\n")
    
    print(f"Summary saved to: {summary_file}")
    
    # Print sample results
    print("\n" + "=" * 80)
    print("SAMPLE RESULTS (first 20 complex equipment items)")
    print("=" * 80)
    for item in complex_equipment[:20]:
        print(f"{item['code']}: {item['description']}")
        print(f"  → {item['reason']}")
        print()

if __name__ == '__main__':
    main()

```
