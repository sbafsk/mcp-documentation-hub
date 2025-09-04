# Business Rules & Domain Logic - Avent Properties

> **AI Context**: This is the single source of truth for business rules.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Business Rules Overview

Avent Properties implements specific business rules to ensure data integrity, user experience, and compliance with business requirements for luxury real estate transactions.

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

#### Property Management Rules
- **Creation**: Only agencies can create property listings
- **Modification**: Only the owning agency can modify properties
- **Deletion**: Properties can only be deleted if no active reservations
- **Access Control**: Available properties visible to all, reserved properties visible to owners only

#### Tour Reservation Rules
- **Workflow**: View → Interest → Tour Booking → Deposit → Tour → Purchase Decision
- **Approval Process**: 10% deposit required for tour booking
- **Status Transitions**: PENDING → CONFIRMED → COMPLETED/CANCELLED

## Domain-Specific Rules

### Real Estate Transaction Rules

#### Business Process
```typescript
interface PropertyTransaction {
  step: 'listing' | 'viewing' | 'reservation' | 'deposit' | 'closing';
  requiredFields: string[];
  allowedTransitions: Record<string, string[]>;
}

const transactionRules = {
  transitions: {
    'listing': ['viewing'],
    'viewing': ['reservation', 'no_interest'],
    'reservation': ['deposit', 'cancellation'],
    'deposit': ['closing', 'refund'],
    'closing': ['completed']
  },
  
  requiredFields: {
    'listing': ['title', 'price', 'city', 'property_type'],
    'reservation': ['scheduled_date', 'deposit_amount'],
    'deposit': ['payment_method', 'amount'],
    'closing': ['legal_docs', 'final_payment']
  }
};
```

#### Validation Logic
```typescript
const validateTransaction = (transaction: PropertyTransaction): ValidationResult => {
  const errors: string[] = [];
  
  // Check required fields
  const required = transactionRules.requiredFields[transaction.step] || [];
  required.forEach(field => {
    if (!transaction[field]) {
      errors.push(`${field} is required for ${transaction.step} step`);
    }
  });
  
  // Check valid transitions
  const currentStep = transaction.step;
  const allowedTransitions = transactionRules.transitions[currentStep] || [];
  if (transaction.nextStep && !allowedTransitions.includes(transaction.nextStep)) {
    errors.push(`Invalid transition from ${currentStep} to ${transaction.nextStep}`);
  }
  
  return {
    isValid: errors.length === 0,
    errors
  };
};
```

### Commission Rules

#### Pricing Rules
```typescript
interface CommissionRule {
  baseCommission: number;
  vatRate: number;
  minimumAmount: number;
  maximumAmount: number;
}

const calculateCommission = (rule: CommissionRule, propertyPrice: number): number => {
  let commission = (rule.baseCommission / 100) * propertyPrice;
  
  // Apply VAT
  const vat = (rule.vatRate / 100) * commission;
  commission += vat;
  
  // Ensure within bounds
  commission = Math.max(commission, rule.minimumAmount);
  commission = Math.min(commission, rule.maximumAmount);
  
  return Math.round(commission * 100) / 100; // Round to 2 decimal places
};
```

## Compliance Rules

### Data Privacy

#### GDPR Compliance
- **Data Retention**: Personal data deleted after 7 years of inactivity
- **Right to Erasure**: Users can request complete data deletion
- **Data Portability**: Users can export their data
- **Consent Management**: Explicit consent required for data processing

#### Data Security
- **Encryption**: All sensitive data encrypted at rest and in transit
- **Access Logging**: All data access logged with user and timestamp
- **Audit Trail**: Complete audit trail for all data modifications
- **Data Classification**: Sensitive data clearly marked and protected

### Regulatory Compliance

#### Real Estate Regulations
- **Uruguay Compliance**: Follow local real estate laws and regulations
- **Financial Regulations**: Comply with financial transaction requirements
- **Tax Compliance**: Proper VAT and commission reporting

#### Industry Standards
- **ISO 27001**: Information security management
- **SOC 2**: Security, availability, and confidentiality
- **PCI DSS**: Payment card industry data security (for Stripe integration)

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
ALTER TABLE properties ADD CONSTRAINT positive_price CHECK (price > 0);

-- Example: Ensure unique emails
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);

-- Example: Ensure valid status transitions
ALTER TABLE tour_reservations ADD CONSTRAINT valid_status_transition 
CHECK (status IN ('pending', 'confirmed', 'cancelled', 'completed'));
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
  if (data.type === 'luxury' && data.price < 1000000) {
    errors.push('Luxury properties must cost at least $1M');
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