# GitHub Issues Creation Checklist

## Overview
This document provides the exact GitHub issues that need to be created for the Hardware Specifications Collection & Management System feature. Each issue includes the machine-readable planner-json block for the C# Developer Agent.

---

## 📋 FEATURE EPIC - Issue #1

**Title**: `[Feature] Hardware Specifications Collection & Management System`  
**Labels**: `feature/epic`, `hardware`, `monitoring`, `backend`, `frontend`, `python-agent`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "featureId": "hardware-specs-system", 
  "type": "feature",
  "title": "Hardware Specifications Collection & Management System",
  "priority": "high",
  "epic": true,
  "businessValue": "Provides comprehensive hardware inventory tracking, theft detection, and capacity planning capabilities",
  "components": ["API", "Application", "Domain", "Persistence", "Infrastructure", "Frontend", "Python Agents"],
  "estimatedEpicSize": "XL (13-21 story points)",
  "childStories": ["STORY-001", "STORY-002", "STORY-003", "STORY-004"],
  "childTasks": [],
  "dependencies": []
}
```

## 📋 Feature Overview
Implement a comprehensive hardware specifications collection and management system that automatically gathers CPU, memory, storage, and motherboard details from monitored machines. This feature provides theft detection through change tracking, capacity planning insights, and configurable collection policies per computer.

## 🎯 Success Criteria
- ✅ **Automatic Collection**: Python agents automatically detect and collect hardware specifications daily
- ✅ **Comprehensive Coverage**: CPU model/cores, RAM capacity/type, storage devices/capacity, motherboard details
- ✅ **Historical Tracking**: Store hardware change history for theft detection and compliance auditing  
- ✅ **Theft Detection**: Alert system when hardware components are removed or changed unexpectedly
- ✅ **Filtering & Views**: Dedicated hardware specifications view with per-machine filtering capabilities
- ✅ **Configurable Collection**: Per-computer settings for collection frequency and component monitoring

## 📊 Estimated Effort
- **Epic Size**: XL (13-21 story points)
- **Timeline**: 3-4 sprints
```

---

## 📚 USER STORY ISSUES

### Issue #2: Story - Automatic Hardware Collection

**Title**: `[Story] As a system administrator, I want automated hardware spec collection`  
**Labels**: `story`, `hardware`, `python-agent`, `backend`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "storyId": "STORY-001",
  "type": "story", 
  "title": "As a system administrator, I want automated hardware spec collection so that I have up-to-date inventory information",
  "priority": "high",
  "parentFeature": "hardware-specs-system",
  "estimation": "5 story points",
  "businessValue": "Eliminates manual hardware inventory processes and ensures data accuracy",
  "files": [
    {
      "path": "src/ResourcesDashboard.Domain/Entities/HardwareSpecification.cs",
      "changeType": "add",
      "suggestion": "New entity with CPU, Memory, Storage, Motherboard value objects"
    },
    {
      "path": "agent/src/hardware_detector.py", 
      "changeType": "add",
      "suggestion": "Hardware detection module using psutil and py-cpuinfo"
    }
  ],
  "dbChanges": {
    "migrations": [
      {
        "name": "AddHardwareSpecificationEntity",
        "expectedTables": ["hardware_specifications"]
      }
    ]
  },
  "tests": ["HardwareSpecificationTests.CreateSpecification_ShouldPersist", "HardwareDetectorTests.DetectCpu_ShouldReturnValidModel"],
  "childTasks": ["TASK-001", "TASK-002", "TASK-003"]
}
```

## User Story
As a system administrator, I want automated hardware specification collection so that I have up-to-date inventory information without manual intervention.

## Acceptance Criteria
- Given a monitored computer with Python agent, when daily collection runs, then CPU model, cores, and architecture are captured
- Given sufficient permissions, when agent scans memory, then total RAM capacity and type (DDR4/DDR5) are detected
- Given system storage access, when collection executes, then all storage devices and their capacities are identified
- Given hardware access, when scanning runs, then motherboard model and manufacturer are captured
- Given configurable collection settings, when frequency is set, then collection respects the configured schedule
- Given network or permission failures, when collection fails, then errors are logged and collection retries gracefully

## Technical Requirements
- **Files to add**: HardwareSpecification.cs, hardware_detector.py
- **Database changes**: Add hardware_specifications table
- **API changes**: New endpoint for hardware data submission from agents
```

### Issue #3: Story - Hardware Specs Dashboard

**Title**: `[Story] As a system administrator, I want a dedicated hardware specs view with filtering`  
**Labels**: `story`, `hardware`, `frontend`, `ui`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "storyId": "STORY-002", 
  "type": "story",
  "title": "As a system administrator, I want a dedicated hardware specs view with filtering so that I can quickly find and compare machine specifications",
  "priority": "high",
  "parentFeature": "hardware-specs-system",
  "estimation": "3 story points",
  "businessValue": "Enables quick hardware comparison, capacity planning, and asset management",
  "files": [
    {
      "path": "frontend/src/pages/HardwareSpecs.tsx",
      "changeType": "add", 
      "suggestion": "New page component with filtering, search, and export capabilities"
    },
    {
      "path": "frontend/src/components/HardwareSpecsTable.tsx",
      "changeType": "add",
      "suggestion": "Reusable table component with sorting and filtering"
    }
  ],
  "apiChanges": [
    {
      "method": "GET",
      "path": "/api/v1/hardware-specs",
      "dto": {"filters": "object", "pagination": "object", "sorting": "object"}
    }
  ],
  "tests": ["HardwareSpecsPage.FilterByCpu_ShouldShowFilteredResults", "HardwareSpecsTable.SortByRam_ShouldOrderCorrectly"],
  "childTasks": ["TASK-004", "TASK-005", "TASK-006"]
}
```

## User Story
As a system administrator, I want a dedicated hardware specifications view with filtering capabilities so that I can quickly find and compare machine specifications for capacity planning and asset management.

## Acceptance Criteria
- Given the hardware specs dashboard, when I apply CPU model filter, then only machines with matching CPUs are displayed
- Given the search functionality, when I search for RAM capacity, then relevant machines are filtered in real-time
- Given the export feature, when I click export, then current filtered data is exported to CSV/Excel
- Given the sorting capability, when I click column headers, then data is sorted appropriately
- Given mobile/tablet access, when I view the dashboard, then it displays properly on smaller screens
```

### Issue #4: Story - Hardware Change Tracking & Theft Detection

**Title**: `[Story] As a security administrator, I want hardware change tracking and theft alerts`  
**Labels**: `story`, `hardware`, `security`, `alerts`, `backend`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "storyId": "STORY-003",
  "type": "story",
  "title": "As a security administrator, I want hardware change tracking and theft alerts so that I can detect unauthorized hardware modifications", 
  "priority": "medium",
  "parentFeature": "hardware-specs-system",
  "estimation": "5 story points",
  "businessValue": "Provides security monitoring and compliance auditing for hardware changes",
  "files": [
    {
      "path": "src/ResourcesDashboard.Domain/Entities/HardwareChangeHistory.cs",
      "changeType": "add",
      "suggestion": "Entity to track hardware component changes over time"
    },
    {
      "path": "frontend/src/components/HardwareHistoryTimeline.tsx",
      "changeType": "add", 
      "suggestion": "Timeline component showing hardware changes with theft detection alerts"
    }
  ],
  "dbChanges": {
    "migrations": [
      {
        "name": "AddHardwareChangeHistoryTable",
        "expectedTables": ["hardware_change_history"]
      }
    ]
  },
  "tests": ["HardwareChangeDetectionTests.DetectRamRemoval_ShouldCreateAlert", "HardwareHistoryService.GetChangeTimeline_ShouldReturnChronologicalChanges"],
  "childTasks": ["TASK-007", "TASK-008", "TASK-009"]
}
```

## User Story
As a security administrator, I want hardware change tracking and theft detection alerts so that I can monitor for unauthorized hardware modifications and maintain security compliance.

## Acceptance Criteria
- Given hardware changes occur, when agents collect specs, then changes are detected and logged
- Given significant changes (missing RAM/storage), when detected, then theft alerts are generated
- Given the history timeline, when I view a computer's history, then all changes are displayed chronologically
- Given false positive management, when I mark a change as legitimate, then it's excluded from future alerts
- Given configurable thresholds, when I set change sensitivity, then detection operates at the specified level
```

### Issue #5: Story - Configurable Collection Settings

**Title**: `[Story] As a system administrator, I want configurable hardware collection settings per computer`  
**Labels**: `story`, `hardware`, `configuration`, `admin`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "storyId": "STORY-004",
  "type": "story", 
  "title": "As a system administrator, I want configurable hardware collection settings per computer so that I can optimize performance and collection frequency",
  "priority": "medium",
  "parentFeature": "hardware-specs-system", 
  "estimation": "3 story points",
  "businessValue": "Allows fine-grained control over collection policies to balance monitoring depth with system performance",
  "files": [
    {
      "path": "src/ResourcesDashboard.Domain/ValueObjects/HardwareCollectionSettings.cs",
      "changeType": "add",
      "suggestion": "Value object encapsulating collection configuration options"
    },
    {
      "path": "frontend/src/components/admin/HardwareCollectionSettings.tsx", 
      "changeType": "add",
      "suggestion": "Settings management UI component with validation"
    }
  ],
  "apiChanges": [
    {
      "method": "PUT",
      "path": "/api/v1/computers/{id}/hardware-collection-settings",
      "dto": {"frequency": "string", "components": "array", "timeWindow": "object"}
    }
  ],
  "tests": ["HardwareCollectionSettingsTests.ValidateFrequency_ShouldRejectInvalidValues", "HardwareCollectionService.ApplySettings_ShouldUpdateCollectionBehavior"],
  "childTasks": ["TASK-010", "TASK-011", "TASK-012"]
}
```

## User Story
As a system administrator, I want configurable hardware collection settings per computer so that I can optimize performance impact and collection frequency based on each machine's role and criticality.

## Acceptance Criteria
- Given computer settings interface, when I set collection frequency, then agents respect the configured schedule
- Given component selection, when I disable storage monitoring, then agents skip storage collection for that computer
- Given time window configuration, when I set collection hours, then agents only collect during specified periods
- Given bulk settings, when I apply settings to multiple computers, then all selected computers are updated
- Given validation rules, when I enter invalid settings, then clear error messages are displayed
```

---

## 🔧 TECHNICAL TASK ISSUES

### Issue #6: Task - Create HardwareSpecification Domain Entity

**Title**: `[Task] Create HardwareSpecification domain entity with value objects`  
**Labels**: `task`, `backend`, `domain`, `entity`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "taskId": "TASK-001",
  "type": "task",
  "title": "Create HardwareSpecification domain entity with value objects",
  "parentStory": "STORY-001", 
  "estimation": "2 story points",
  "priority": "high",
  "files": [
    {
      "path": "src/ResourcesDashboard.Domain/Entities/HardwareSpecification.cs",
      "changeType": "add",
      "suggestion": "public class HardwareSpecification : BaseEntity\n{\n    public Guid ComputerId { get; private set; }\n    public CpuSpecification Cpu { get; private set; }\n    public MemorySpecification Memory { get; private set; }\n    public StorageSpecifications Storage { get; private set; }\n    public MotherboardSpecification Motherboard { get; private set; }\n    public DateTime CollectedAt { get; private set; }\n    public DateTime? PreviousCollectionAt { get; private set; }\n}"
    },
    {
      "path": "src/ResourcesDashboard.Domain/ValueObjects/CpuSpecification.cs",
      "changeType": "add", 
      "suggestion": "public class CpuSpecification : ValueObject\n{\n    public string Model { get; private set; }\n    public string Manufacturer { get; private set; }\n    public int Cores { get; private set; }\n    public int Threads { get; private set; }\n    public string Architecture { get; private set; }\n    public double? ClockSpeedGhz { get; private set; }\n}"
    }
  ],
  "tests": [
    "HardwareSpecificationTests.Create_WithValidData_ShouldSucceed",
    "CpuSpecificationTests.Equals_WithSameValues_ShouldReturnTrue",
    "MemorySpecificationTests.Create_WithInvalidCapacity_ShouldThrowException"
  ]
}
```

## Task Description
Create the HardwareSpecification domain entity and associated value objects (CpuSpecification, MemorySpecification, StorageSpecifications, MotherboardSpecification) following Clean Architecture principles.

## Files to Change
- `src/ResourcesDashboard.Domain/Entities/HardwareSpecification.cs` - Main entity class
- `src/ResourcesDashboard.Domain/ValueObjects/CpuSpecification.cs` - CPU specification value object
- `src/ResourcesDashboard.Domain/ValueObjects/MemorySpecification.cs` - Memory specification value object  
- `src/ResourcesDashboard.Domain/ValueObjects/StorageSpecifications.cs` - Storage specifications collection
- `src/ResourcesDashboard.Domain/ValueObjects/MotherboardSpecification.cs` - Motherboard specification value object

## Acceptance Criteria
- ✅ HardwareSpecification entity follows Clean Architecture principles with private setters
- ✅ All value objects are immutable with proper equality comparison
- ✅ Entity includes audit fields (CreatedAt, UpdatedAt) from BaseEntity
- ✅ Relationship to Computer entity is properly configured
- ✅ Value objects validate input data and throw descriptive exceptions for invalid data

## Technical Details
```csharp
// Example implementation structure
public class HardwareSpecification : BaseEntity
{
    public Guid ComputerId { get; private set; }
    public CpuSpecification Cpu { get; private set; }
    public MemorySpecification Memory { get; private set; }
    public StorageSpecifications Storage { get; private set; }
    public MotherboardSpecification Motherboard { get; private set; }
    public DateTime CollectedAt { get; private set; }
    
    // Factory method
    public static HardwareSpecification Create(Guid computerId, CpuSpecification cpu, MemorySpecification memory, StorageSpecifications storage, MotherboardSpecification motherboard)
    {
        // Implementation with validation
    }
}
```

## Tests Required
- ✅ Unit tests for HardwareSpecification entity creation and validation
- ✅ Unit tests for all value objects with equality and validation testing
- ✅ Integration tests for entity persistence with EF Core

**Estimated Complexity**: 2 Story Points  
**Risk Level**: Low
```

### Issue #7: Task - Implement Hardware Detection Python Module

**Title**: `[Task] Implement hardware detection module in Python agent`  
**Labels**: `task`, `python-agent`, `hardware-detection`

**Body**:
```markdown
```planner-json
{
  "plannerJsonVersion": 1,
  "taskId": "TASK-002",
  "type": "task",
  "title": "Implement hardware detection module in Python agent",
  "parentStory": "STORY-001",
  "estimation": "3 story points", 
  "priority": "high",
  "files": [
    {
      "path": "agent/src/hardware_detector.py",
      "changeType": "add",
      "suggestion": "import psutil\nimport platform\nfrom cpuinfo import get_cpu_info\n\nclass HardwareDetector:\n    def detect_cpu(self):\n        cpu_info = get_cpu_info()\n        return {\n            'model': cpu_info.get('brand_raw', 'Unknown'),\n            'manufacturer': cpu_info.get('vendor_id_raw', 'Unknown'),\n            'cores': psutil.cpu_count(logical=False),\n            'threads': psutil.cpu_count(logical=True),\n            'architecture': platform.machine()\n        }"
    },
    {
      "path": "agent/requirements.txt",
      "changeType": "modify",
      "suggestion": "Add: py-cpuinfo>=9.0.0\npsutil>=5.9.0 (already present)"
    }
  ],
  "tests": [
    "test_hardware_detector.py::test_detect_cpu_returns_valid_data",
    "test_hardware_detector.py::test_detect_memory_handles_insufficient_permissions",
    "test_hardware_detector.py::test_detect_storage_enumerates_all_devices"
  ]
}
```

## Task Description
Create a comprehensive hardware detection module that can identify CPU, memory, storage, and motherboard specifications across Windows, Linux, and macOS platforms.

## Files to Change
- `agent/src/hardware_detector.py` - New hardware detection module
- `agent/requirements.txt` - Add py-cpuinfo dependency
- `agent/tests/test_hardware_detector.py` - Unit tests for hardware detection

## Acceptance Criteria
- ✅ Cross-platform detection works on Windows, Linux, and macOS
- ✅ CPU model, manufacturer, cores, threads, and architecture detected
- ✅ Memory capacity and type (DDR3/DDR4/DDR5) identified where possible
- ✅ Storage devices enumerated with capacity and interface type
- ✅ Motherboard model and manufacturer captured when available
- ✅ Graceful handling of detection failures with appropriate logging

## Technical Details
```python
# Example implementation structure
class HardwareDetector:
    def __init__(self):
        self.logger = logging.getLogger(__name__)
    
    def detect_all_hardware(self):
        return {
            'cpu': self.detect_cpu(),
            'memory': self.detect_memory(),
            'storage': self.detect_storage(),
            'motherboard': self.detect_motherboard()
        }
    
    def detect_cpu(self):
        # Implementation using py-cpuinfo and psutil
        pass
```

## Tests Required
- ✅ Unit tests for each detection method with mocked system calls
- ✅ Integration tests on different operating systems
- ✅ Error handling tests for insufficient permissions

**Estimated Complexity**: 3 Story Points  
**Risk Level**: Medium (cross-platform compatibility)
```

### Additional Task Issues (8-12)
[Continue with TASK-003 through TASK-012 following the same pattern...]

---

## 🎯 CREATION INSTRUCTIONS

1. **Create these issues in order** (Feature → Stories → Tasks)
2. **Use exact titles and labels** as specified above
3. **Copy planner-json blocks exactly** - the C# Developer Agent relies on this format
4. **Link child issues to parents** using GitHub's issue linking (#issue-number)
5. **Set appropriate milestones** and assign to team members as needed

Each issue contains comprehensive implementation guidance that follows the existing ResourcesDashboard codebase patterns and Clean Architecture principles.