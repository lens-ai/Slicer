# LumoScan - 3D Body Scanning & Lesion Analysis System

## Project Overview

**Project Name**: LumoScan  
**Type**: End-to-End 3D Medical Imaging & AI Analysis Platform  
**Classification**: Medical Device Software - Class II  
**Regulatory Framework**: FDA 21 CFR Part 820, FDA Section 524B  
**Deployment Status**: Clinical deployment in dermatology clinics and hospital settings  
**License**: Proprietary

## System Description

LumoScan is a medical-grade 3D body scanning and lesion analysis system designed for dermatological clinical environments. The system captures high-resolution depth imagery using stereo sensors, processes images through cloud-based AI modules for lesion classification and evolution tracking, and delivers results to clinicians via a browser-based DICOM-compatible viewer.

**Mission**: Enable early detection and monitoring of skin lesions through advanced 3D imaging and AI-powered analysis, improving dermatological care outcomes.

## Core Features

### Integration Capabilities
- **DICOM-Native Architecture**: Full DICOM compliance for seamless integration with existing medical imaging infrastructure
- **EHR Integration Ready**: Planned integration with Epic and Oracle Health (Cerner) systems
- **Cloud-Based AI Processing**: Scalable AI analysis via Google Cloud Platform
- **Multi-Output Support**: 2D widefield images, 3D renderings, and AI-enriched annotations

### Deployment Flexibility
- **Multi-Environment Support**: On-premise scanner hardware with cloud AI processing
- **Hybrid Architecture**: Local capture with cloud analysis and provider-controlled storage
- **Platform-Agnostic Viewing**: Browser-based viewer accessible from any device

## Technical Architecture

### AI/ML Models

#### Core AI Components
| Model | Framework | Purpose | Deployment |
|-------|-----------|---------|------------|
| Calibration AI | OpenCV / scikit-learn | Device calibration, sensor alignment | On-device |
| LumoScreen | TensorFlow / PyTorch | Lesion classification | Cloud (GCP) |
| LumoTrack | TensorFlow / PyTorch | Lesion evolution tracking | Cloud (GCP) |
| LumoCrunch | OpenCV / PyTorch | Image preprocessing, blob detection, 3D rendering | Cloud (GCP) |

#### Model Specifications
| Model Type | Format | Framework | Deployment |
|------------|--------|-----------|------------|
| Calibration Model | .pkl | OpenCV / scikit-learn | On-device |
| Classification Model | .tflite | TensorFlow Lite | Cloud |
| Evolution Model | .tflite | TensorFlow Lite | Cloud |
| Configuration | .json | N/A | Both |

### Technical Stack

**Scanner Hardware Environment**:
- Ubuntu Linux
- ROS (Robot Operating System)
- C++, Python
- Stereo depth sensors + 2D camera
- 64 GB RAM, 1 TB HDD
- 6 pan/tilt motors + 1 linear motion motor
- PLC controller with ModBUS protocol

**Cloud Environment**:
- Google Cloud Platform (US-East)
- Compute Engine for AI processing
- Cloud Storage for training data
- Cloud SQL for relational data
- Cloud KMS for encryption key management

**Key Dependencies**:
- OpenCV - Image processing and calibration
- PyTorch - Deep learning inference
- TensorFlow / TensorFlow Lite - AI model deployment
- React - Web UI framework (LumoDoc viewer)
- DICOM.js - DICOM file handling
- Terraform - Infrastructure as Code

**Infrastructure**:
- Multi-zone GCP deployment
- HTTPS/TLS for all API communications
- SSO integration with hospital Active Directory
- DICOM-standard security for image exchange

## Functional Modules

### Two Core Capabilities

1. **3D Imaging & Capture System**
   - High-resolution stereo depth imaging
   - 2D widefield dermascopic capture
   - Automated calibration and sensor alignment
   - Real-time 3D body surface rendering

2. **AI Analysis Pipeline**
   - Automated lesion detection and classification (LumoScreen)
   - Longitudinal lesion evolution tracking (LumoTrack)
   - DICOM-enriched output with annotations
   - Clinical decision support for dermatologists

### Complete Feature Matrix

| Function | Process Stage | Type | Target Users | Output |
|----------|---------------|------|--------------|--------|
| 3D Body Scanning | Image Capture | Hardware + Software | Technicians | 2D Widefield Images |
| Device Calibration | Setup | Software | Technicians | Calibration Parameters |
| Image Preprocessing | Processing | Cloud AI | System | Blob Detection Results |
| 3D Rendering | Processing | Cloud AI | System | 3D DICOM Files |
| Lesion Classification | Analysis | Cloud AI | Clinicians | DICOM + Annotations |
| Evolution Tracking | Analysis | Cloud AI | Clinicians | Enriched DICOM Files |
| DICOM Viewing | Clinical Review | Web Application | Clinicians | HTML Rendered View |
| Measurement & Annotation | Clinical Review | Web Application | Clinicians | Annotated DICOM |
| Invoice Generation | Administrative | Cloud Service | Administrators | Invoice & Test Results |

## Technical Advantages

### AI Capabilities
- **Multi-Model Pipeline**: Specialized models for calibration, classification, and evolution tracking
- **Cloud-Native Scaling**: GCP infrastructure enables elastic scaling for high-volume processing
- **DICOM-Enriched Output**: AI results embedded directly in DICOM metadata for seamless clinical workflow

### Key Challenge Solutions
- Stereo sensor calibration for accurate 3D reconstruction
- Blob detection for automated lesion identification
- Longitudinal tracking across multiple patient visits
- DICOM compliance for interoperability with existing systems

### Clinical Experience
- **Browser-Based Viewer**: Platform-agnostic access to all imaging and AI results
- **EHR Integration Ready**: Designed for Epic and Oracle Health integration
- **SSO Support**: Hospital Active Directory integration via SAML 2.0 / OAuth 2.0
- **Annotation Tools**: Clinician markup capabilities with DICOM persistence

## Security & Compliance

### Data Processing
- Ephemeral processing in LumoCrunch (data deleted after session)
- Provider-controlled DICOM Store for PHI persistence
- De-identified training dataset maintained separately
- Google Cloud security controls and IAM

### Protected Health Information (PHI)

**Image Data**:
- 2D widefield dermascopic images
- 3D body surface scans
- Processed DICOM files with annotations
- Lesion classification results
- Lesion evolution tracking data

**Demographic Data** (10 fields):
- Patient identifier (MRN)
- Patient age
- Patient gender
- Additional demographic fields per facility configuration

### Data Storage Locations

| Environment | Storage Location | Data Types | Retention |
|-------------|------------------|------------|-----------|
| Development | Google Cloud Storage | Test images, synthetic data | Per dev lifecycle |
| Production | Provider's DICOM Store (On-premise/EHR) | Patient images, demographics, AI results | Per facility policy |
| AI Training | Lumo's Training Dataset (GCP) | De-identified dermascopic images + annotations | Long-term |
| Processing | LumoCrunch (ephemeral) | In-transit scan data | Deleted after processing |

### Usage Restrictions
- **Regulatory Compliance**: FDA 21 CFR Part 820, FDA Section 524B
- **Clinical Use Only**: Intended for licensed dermatology professionals
- **Prohibited Uses**:
  - Non-clinical or consumer applications
  - Use without proper training and certification

### Security Limitations & Planned Improvements
- Firmware encryption (not currently implemented)
- End-to-end encryption for PHI at rest (planned)
- Comprehensive audit logging (under development)
- Vulnerability management program (under development)
- OTA firmware updates (not currently planned)

## Service Architecture

### Scanner Service (lumoscan-scanner)
```bash
# ROS-based scanner operation
roslaunch lumoscan scanner.launch \
  --device-id <scanner_id> \
  --calibration-path /path/to/calibration \
  --output-endpoint https://<lumcrunch_endpoint>/upload
```

**Key Components**:
- ROS nodes for sensor control
- Device drivers for stereo sensors
- Motor control via ModBUS protocol
- Image capture and upload pipeline

### Cloud Services (lumoscan-cloud)

**LumoCrunch** - Image Preprocessing:
- Input: 2D widefield images from scanner
- Processing: Blob detection, 3D rendering
- Output: 2D Widefield + 3D Rendering DICOM files
- Destination: Provider's DICOM Store

**LumoScreen** - Lesion Classification:
- Input: DICOM images from Provider's DICOM Store
- Processing: AI-powered lesion classification
- Output: Enriched DICOM + Dermascopic + Annotations
- Additional Output: Invoice & Test Results

**LumoTrack** - Evolution Tracking:
- Input: DICOM images from Provider's DICOM Store
- Processing: Longitudinal lesion analysis
- Output: 2 DICOM files with evolution data
- Additional Output: Invoice & Test Results

### Web UI Service (lumoscan-ui / lumoscan-viewer)
```bash
# LumoDoc viewer deployment
npm run build
npm run start -- --port <webui_port>
```

**Access**: `https://<viewer_domain>:<port>`

## Data Flow Architecture

### Primary Data Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         LumoScan Data Flow                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────────┐                                                        │
│  │  LumoScan        │                                                        │
│  │  Hardware        │──(1) 2D Widefield Images──►┌──────────────────┐       │
│  │  (Scanner)       │                            │   LumoCrunch     │       │
│  └──────────────────┘                            │  (Preprocessing) │       │
│                                                  └────────┬─────────┘       │
│                                                           │                  │
│                                        (2) 2D Widefield + 3D Rendering       │
│                                                           │                  │
│                                                           ▼                  │
│                                              ┌────────────────────────┐      │
│  ┌──────────────────┐    (3) DICOM          │   Provider's DICOM     │      │
│  │    LumoDoc       │◄──────────────────────│        Store           │      │
│  │    (Viewer)      │                       │   (Central Storage)    │      │
│  └────────┬─────────┘                       └───────────┬────────────┘      │
│           │                                      │             │             │
│  (4) HTML to Browser                            │             │             │
│           │                          (5) DICOM  │             │ (8) DICOM   │
│           ▼                                     ▼             ▼             │
│  ┌──────────────────┐               ┌─────────────┐  ┌─────────────┐        │
│  │    Clinician     │               │ LumoScreen  │  │  LumoTrack  │        │
│  │    Browser       │               │(Classify)   │  │ (Evolution) │        │
│  └──────────────────┘               └──────┬──────┘  └──────┬──────┘        │
│                                            │                 │               │
│                              (6) DICOM +   │                 │ (9) 2 DICOM  │
│                              Annotations   │                 │    Files     │
│                                            ▼                 ▼               │
│                                     Back to DICOM Store                      │
│                                                                              │
│                              (7,10) Invoice & Test Results → External        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Data Flow Steps

**Image Capture & Processing**:
1. LumoScan hardware captures 2D widefield images
2. Images uploaded to LumoCrunch for preprocessing
3. LumoCrunch performs blob detection and 3D rendering
4. Processed DICOM files stored to Provider's DICOM Store

**AI Analysis Pipeline**:
5. DICOM Store sends images to LumoScreen
6. LumoScreen performs lesion classification
7. LumoScreen returns enriched DICOM + Annotations to DICOM Store
8. DICOM Store sends images to LumoTrack
9. LumoTrack performs evolution analysis
10. LumoTrack returns 2 enriched DICOM files to DICOM Store
11. Both AI modules send Invoice & Test Results externally

**Clinical Viewing**:
12. LumoDoc retrieves DICOM images from Provider's DICOM Store
13. LumoDoc renders HTML to clinician's browser
14. Clinician views images, measurements, and AI analysis results
15. Clinician annotations saved back to DICOM Store

### Data Flow Matrix

| Source | Destination | Data Type | Protocol |
|--------|-------------|-----------|----------|
| LumoScan Hardware | LumoCrunch | 2D Widefield Images | REST API / HTTPS |
| LumoCrunch | DICOM Store | 2D Widefield + 3D Rendering Data | DICOM |
| DICOM Store | LumoDoc | DICOM Images | DICOM |
| LumoDoc | DICOM Store | DICOM + Dermoscopic + Annotations | DICOM |
| DICOM Store | LumoScreen | DICOM Images | DICOM |
| LumoScreen | DICOM Store | DICOM + Dermoscopic + Annotations | DICOM |
| DICOM Store | LumoTrack | DICOM Images | DICOM |
| LumoTrack | DICOM Store | 2 DICOM Files (enriched) | DICOM |
| LumoScreen | External | Invoice & Test Results | API |
| LumoTrack | External | Invoice & Test Results | API |
| LumoDoc | Browser | HTML Rendered View | HTTPS |
| Training Dataset | LumoScreen/LumoTrack | Dermascopic Images + Annotations | Internal |

## Testing & Validation

### Repository Structure

| Repository | Contents | Primary Language |
|------------|----------|------------------|
| lumoscan-scanner | ROS nodes, firmware, device drivers | C++, Python |
| lumoscan-calibration | Calibration scripts and ML models | Python |
| lumoscan-ui | Scanning user interface | React / JavaScript |
| lumoscan-viewer (fork) | LumoDoc - DICOM viewer | React / JavaScript |
| lumoscan-cloud | LumoCrunch, LumoScreen, LumoTrack + Infrastructure | Python, Terraform |

### Validation Areas
1. Scanner calibration accuracy
2. 3D reconstruction fidelity
3. Lesion classification performance
4. Evolution tracking accuracy
5. DICOM compliance verification
6. EHR integration testing (future)

## Intended Use

### Clinical Environment
- **Primary**: Dermatology clinics and hospital clinical settings
- **Secondary**: Telemedicine dermatology consultations
- **Tertiary**: Clinical research studies

### Patient Population
- All patient demographics requiring dermatological assessment
- Skin lesion monitoring patients
- Skin cancer screening candidates

### User Roles

| Role | Description | Primary Functions |
|------|-------------|-------------------|
| Clinician | Dermatologists, healthcare providers | View scans, review AI classifications, clinical interpretation |
| Technician | Scanner operators | Conduct scans, device operation, image capture |
| Administrator | System managers | User management, configuration, system monitoring |
| Researcher | Clinical researchers | Access de-identified data for approved studies |

### Access Control
- **Clinicians**: Full patient data access including AI results
- **Technicians**: Scan operation, limited patient data
- **Administrators**: Full system access, user management
- **Researchers**: Aggregate/de-identified data access only

### Authentication
- Primary Method: SSO with Hospital Active Directory
- Protocol: SAML 2.0 / OAuth 2.0
- Session Management: Token-based with configurable timeout
- MFA: Supported per hospital policy

## External Integrations

### Third-Party Services

| Service | Provider | Purpose | Data Exchanged |
|---------|----------|---------|----------------|
| Cloud Compute | Google Cloud Platform | AI processing | DICOM images, AI results |
| Cloud Storage | Google Cloud Storage | Training dataset, ephemeral processing | Images, annotations |
| Authentication | Google OpenID Connect | User SSO | Identity tokens |
| EHR (Future) | Epic / Oracle Health | Patient data integration | Demographics, DICOM images |

### APIs Exposed
- **Scan Upload API**: Receive 2D widefield images from Scanner → LumoCrunch
- **DICOM API**: Integration with Provider's DICOM Store
- **Viewer API**: Serve processed images to LumoDoc
- **AI Results API**: Return classification and evolution results
- **Invoice API**: Return billing/test results from LumoScreen and LumoTrack

### Hardware Connectivity

| Connection | Interface | Components | Protocol |
|------------|-----------|------------|----------|
| USB | Direct | Cameras, Depth Sensors, Motors | USB 3.0 |
| Ethernet | Network | PLC Controller | ModBUS TCP/IP |
| REST API | Cloud | LumoCrunch Backend | HTTPS |

## Areas for FDA 524B Compliance Review

- SBOM generation for all software components (Scanner, LumoCrunch, LumoDoc, LumoScreen, LumoTrack)
- Vulnerability disclosure and management procedures
- Third-party component security assessment
- AI model security (adversarial input protection)
- Firmware update security and integrity verification
- PHI encryption at rest and in transit
- Training data security and privacy

## Repository Information

**Repositories**:
- lumoscan-scanner: Scanner firmware and ROS nodes
- lumoscan-calibration: Calibration scripts
- lumoscan-ui: Scanner interface
- lumoscan-viewer: LumoDoc DICOM viewer
- lumoscan-cloud: Cloud services and infrastructure

**Infrastructure**: Terraform-managed GCP deployment

## Development Roadmap

### Current Status
- ✅ 3D body scanning hardware operational
- ✅ Cloud-based AI processing pipeline (LumoCrunch, LumoScreen, LumoTrack)
- ✅ DICOM-compliant storage and viewing
- ✅ SSO integration with hospital Active Directory
- ✅ Browser-based clinical viewer (LumoDoc)

### Planned Development
- ⏳ Epic / Oracle Health EHR integration
- ⏳ Firmware encryption implementation
- ⏳ End-to-end PHI encryption at rest
- ⏳ Comprehensive audit logging
- ⏳ Vulnerability management program
- ⏳ FDA 524B compliance documentation

### Contact & Support
- **Technical Issues**: Engineering team
- **Clinical Support**: Clinical applications team
- **Hospital Deployments**: Implementation services
- **Regulatory**: Regulatory affairs team


**Last Updated**: December 2025  
**Document Version**: 1.0  
**Maintained By**: LumoScan Development Team
