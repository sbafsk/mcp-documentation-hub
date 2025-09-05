# Business Rules & Domain Logic

> **AI Context**: This is the single source of truth for business rules.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Business Rules Overview

[Project Name] implements specific business rules to ensure data integrity, user experience, and compliance with business requirements.

## Core Business Rules

### User Management

#### User Registration
- **Email Validation**: Must be unique across the system
- **Password Requirements**: Minimum 8 characters, must include uppercase, lowercase, number, and special character
- **Age Requirement**: Users must be 18+ years old
- **Verification**: Email verification required before account activation

#### User Authentication
- **Session Duration**: 24 hours for regular users, 7 days for "remember me"
- **Failed Login Attempts**: Account locked after 5 failed attempts for 30 minutes
- **Password Reset**: Token expires after 1 hour
- **Multi-factor Authentication**: Required for admin accounts

### Data Validation Rules

#### Input Validation
- **String Lengths**: 
  - Names: 2-50 characters
  - Descriptions: 0-1000 characters
  - URLs: Valid URL format
- **Number Ranges**: 
  - Prices: Positive numbers only
  - Quantities: Non-negative integers
  - Percentages: 0-100 range

#### Data Sanitization
- **HTML Content**: Strip potentially harmful HTML tags
- **File Uploads**: Validate file types and sizes
- **SQL Injection**: Use parameterized queries only
- **XSS Prevention**: Encode user input before rendering

### Business Logic Rules

#### [Feature 1] Rules
- **Creation**: [Rule for creating items]
- **Modification**: [Rule for modifying items]
- **Deletion**: [Rule for deleting items]
- **Access Control**: [Who can access what]

#### [Feature 2] Rules
- **Workflow**: [Step-by-step workflow rules]
- **Approval Process**: [Approval requirements]
- **Status Transitions**: [Valid status changes]

## Domain-Specific Rules

### [Domain 1] Rules

#### Business Process
```typescript
interface BusinessProcess {
  // Define the business process structure
  step: 'initiation' | 'review' | 'approval' | 'completion';
  requiredFields: string[];
  allowedTransitions: Record<string, string[]>;
}

const businessRules = {
  // Define valid transitions
  transitions: {
    'initiation': ['review'],
    'review': ['approval', 'rejection'],
    'approval': ['completion'],
    'rejection': ['initiation']
  },
  
  // Define required fields for each step
  requiredFields: {
    'initiation': ['title', 'description'],
    'review': ['reviewer_notes'],
    'approval': ['approver_signature'],
    'completion': ['completion_date']
  }
};
```

#### Validation Logic
```typescript
const validateBusinessProcess = (process: BusinessProcess): ValidationResult => {
  const errors: string[] = [];
  
  // Check required fields
  const required = businessRules.requiredFields[process.step] || [];
  required.forEach(field => {
    if (!process[field]) {
      errors.push(`${field} is required for ${process.step} step`);
    }
  });
  
  // Check valid transitions
  const currentStep = process.step;
  const allowedTransitions = businessRules.transitions[currentStep] || [];
  if (process.nextStep && !allowedTransitions.includes(process.nextStep)) {
    errors.push(`Invalid transition from ${currentStep} to ${process.nextStep}`);
  }
  
  return {
    isValid: errors.length === 0,
    errors
  };
};
```

### [Domain 2] Rules

#### Pricing Rules
```typescript
interface PricingRule {
  basePrice: number;
  discountPercentage: number;
  minimumQuantity: number;
  maximumDiscount: number;
}

const calculatePrice = (rule: PricingRule, quantity: number): number => {
  let finalPrice = rule.basePrice * quantity;
  
  // Apply quantity discount
  if (quantity >= rule.minimumQuantity) {
    const discount = (rule.discountPercentage / 100) * finalPrice;
    const maxDiscount = (rule.maximumDiscount / 100) * finalPrice;
    finalPrice -= Math.min(discount, maxDiscount);
  }
  
  return Math.max(finalPrice, 0); // Ensure non-negative price
};
```

## Compliance Rules

### Data Privacy

#### GDPR Compliance
- **Data Retention**: Personal data deleted after [X] years of inactivity
- **Right to Erasure**: Users can request complete data deletion
- **Data Portability**: Users can export their data
- **Consent Management**: Explicit consent required for data processing

#### Data Security
- **Encryption**: All sensitive data encrypted at rest and in transit
- **Access Logging**: All data access logged with user and timestamp
- **Audit Trail**: Complete audit trail for all data modifications
- **Data Classification**: Sensitive data clearly marked and protected

### Regulatory Compliance

#### Industry Standards
- **ISO 27001**: Information security management
- **SOC 2**: Security, availability, and confidentiality
- **PCI DSS**: Payment card industry data security (if applicable)

#### Legal Requirements
- **Data Localization**: Data stored in [specific regions] only
- **Reporting Requirements**: Regular compliance reports generated
- **Incident Response**: 24-hour notification for data breaches

## Workflow Rules

### Approval Workflows

#### Multi-Level Approval
```typescript
interface ApprovalWorkflow {
  levels: ApprovalLevel[];
  currentLevel: number;
  status: 'pending' | 'approved' | 'rejected';
}

interface ApprovalLevel {
  level: number;
  approverRole: string;
  required: boolean;
  autoApprove: boolean;
}

const processApproval = (workflow: ApprovalWorkflow, decision: 'approve' | 'reject'): ApprovalWorkflow => {
  if (decision === 'reject') {
    return { ...workflow, status: 'rejected' };
  }
  
  const nextLevel = workflow.currentLevel + 1;
  if (nextLevel >= workflow.levels.length) {
    return { ...workflow, status: 'approved' };
  }
  
  return { ...workflow, currentLevel: nextLevel };
};
```

### Business Process Automation

#### Trigger Rules
- **Time-based**: Automatic actions at specific times
- **Event-based**: Actions triggered by specific events
- **Condition-based**: Actions when conditions are met

#### Action Rules
- **Notifications**: Automatic email/SMS notifications
- **Status Updates**: Automatic status changes
- **Data Processing**: Automatic data transformations

## Exception Handling

### Business Exceptions

#### Custom Exception Types
```typescript
class BusinessRuleViolation extends Error {
  constructor(
    message: string,
    public rule: string,
    public context: Record<string, any>
  ) {
    super(message);
    this.name = 'BusinessRuleViolation';
  }
}

class ValidationError extends Error {
  constructor(
    message: string,
    public field: string,
    public value: any
  ) {
    super(message);
    this.name = 'ValidationError';
  }
}
```

#### Exception Handling Strategy
```typescript
const handleBusinessException = (error: Error): void => {
  if (error instanceof BusinessRuleViolation) {
    // Log business rule violation
    logger.warn('Business rule violated', {
      rule: error.rule,
      context: error.context
    });
    
    // Notify relevant stakeholders
    notifyStakeholders(error);
  } else if (error instanceof ValidationError) {
    // Handle validation errors
    logger.error('Validation failed', {
      field: error.field,
      value: error.value
    });
  } else {
    // Handle unexpected errors
    logger.error('Unexpected error', { error: error.message });
  }
};
```

## Rule Enforcement

### Implementation Methods

#### Database Constraints
```sql
-- Example: Ensure positive prices
ALTER TABLE products ADD CONSTRAINT positive_price CHECK (price > 0);

-- Example: Ensure unique emails
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);

-- Example: Ensure valid status transitions
ALTER TABLE orders ADD CONSTRAINT valid_status_transition 
CHECK (status IN ('pending', 'processing', 'shipped', 'delivered'));
```

#### Application-Level Validation
```typescript
const validateBusinessRules = (data: any): ValidationResult => {
  const errors: string[] = [];
  
  // Apply business rules
  if (data.price && data.price <= 0) {
    errors.push('Price must be positive');
  }
  
  if (data.email && !isValidEmail(data.email)) {
    errors.push('Invalid email format');
  }
  
  // Apply domain-specific rules
  if (data.type === 'premium' && data.price < 100) {
    errors.push('Premium items must cost at least $100');
  }
  
  return {
    isValid: errors.length === 0,
    errors
  };
};
```

### Monitoring & Auditing

#### Rule Compliance Monitoring
- **Real-time Validation**: Validate rules on every operation
- **Compliance Reports**: Regular reports on rule compliance
- **Exception Tracking**: Track and analyze rule violations
- **Performance Impact**: Monitor rule enforcement performance

#### Audit Trail
- **Rule Changes**: Track all business rule modifications
- **Violation History**: Maintain history of rule violations
- **Compliance History**: Track compliance over time
- **User Actions**: Log all user actions for audit purposes

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Current Status](../docs/status/progress.yaml)
- [Coding Standards](coding.md)
- [Architectural Patterns](patterns.md)