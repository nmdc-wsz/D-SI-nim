# D-SI-nim: A Machine-interpretable and traceable D-SI meta model

A Digital-SI meta model with machine interpretability and traceability capabilities, enabling automated parsing,
validation, and authoritative traceability of units in Digital Calibration Certificates (DCCs).

## Overview

This repository presents a Digital-SI (D-SI) meta model (version: `D-SI-nim-v2.2.0`) designed to address the
critical limitations of existing D-SI frameworks in machine interpretability and traceability.

By integrating a Non-deterministic Finite Automaton (NFA) for syntax parsing and a Metadata Description Framework (MDF)
linked to the SI Reference Point (SIRP), this model enables:

- Automated validation of unit expression rules (e.g., correct order of prefix, unit, and exponent)
- Semantic constraint checking aligned with the SI Brochure
- Direct traceability to authoritative digital definitions via SIRP

It serves as a foundational tool for advancing Digital Calibration Certificates (DCCs) and digital transformation for
metrology.

## Key Features

### 1. Machine-interpretable

- NFA-based Syntax Validation: Formalizes D-SI unit expression rules (prefix + unit + exponent structure) into an
  automaton model, enabling automated parsing and error detection (e.g., invalid combinations like `\kilo\gram`).
- Semantic Understanding: Embeds logic to recognize derived units (e.g., `\kilogram\metre\second\tothe{-2}`
  as `\newton`)
  and enforce SI Brochure constraints (e.g., 10<sup>-6</sup> kg must be mg instead of µkg).

### 2. Machine-traceable

- SIRP Integration: Links unit metadata to the BIPM's SI Reference Point (SIRP), a digital authoritative source for SI
  units, via persistent identifiers (PIDs).
- Metadata Framework: Standardizes fields like dsiId (D-SI identifier), sirpPID (SIRP link), and dsiBaseUnitExpression (
  base unit decomposition) for traceability.

## Usage

### Python

```python
import lxml.etree as ET


def validateXMLByXSD(file_xml, file_xsd):
    """ Verify that the XML compliance with XSD
    Arguments:
        1. file_xml: Input xml file
        2. file_xsd: xsd file which needs to be validated against xml
    Return:
        No return value
    """
    try:
        print("Validating:{0}".format(file_xml))
        print("xsd_file:{0}".format(file_xsd))
        xml_doc = ET.parse(file_xml)
        xsd_doc = ET.parse(file_xsd)
        xmlschema = ET.XMLSchema(xsd_doc)
        xmlschema.assert_(xml_doc)
        return True

    except ET.XMLSyntaxError as err:
        print("PARSING ERROR:{0}".format(err))
        return False

    except AssertionError as err:
        print("Incorrect XML schema: {0}".format(err))
        return False
```

For wrong expression `\micro\kilogram`:

```
Incorrect XML schema: Element '{https://www.nim.ac.cn/si}unit': [facet 'pattern'] The value '\micro\kilogram' is not accepted by the pattern ...
```

## Related Links

1. https://gitlab1.ptb.de/d-ptb/d-si
2. https://si-digital-framework.org/
3. https://www.nmdc.ac.cn/main/#/pages/index
