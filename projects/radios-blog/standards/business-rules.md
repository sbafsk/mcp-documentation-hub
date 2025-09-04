# Business Rules & Domain Logic - Radios Blog

> **AI Context**: This is the single source of truth for business rules.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Business Rules Overview

Radios Blog implements specific business rules to ensure data integrity, user experience, and compliance with business requirements for vintage radio collections, blog content, and postcard galleries.

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
  - Names: 2-100 characters
  - Descriptions: 0-2000 characters
  - URLs: Valid URL format
- **Number Ranges**: 
  - Prices: Positive numbers only (0.01 - 999,999.99)
  - Years: 1900 - current year
  - Quantities: Non-negative integers

#### Data Sanitization
- **HTML Content**: Strip potentially harmful HTML tags
- **File Uploads**: Validate file types and sizes
- **SQL Injection**: Use parameterized queries only
- **XSS Prevention**: Encode user input before rendering

### Business Logic Rules

#### Vintage Radio Management Rules
- **Creation**: Only authenticated users can create radio listings
- **Modification**: Only the listing owner can modify radio details
- **Deletion**: Radios can only be deleted if no active transactions
- **Access Control**: Available radios visible to all, sold radios visible to owners only

#### Blog Post Rules
- **Creation**: Only admin users can create blog posts
- **Modification**: Only the author can modify their posts
- **Publication**: Posts require admin approval before publication
- **Content Standards**: Must meet quality and accuracy guidelines

#### Postcard Gallery Rules
- **Curated Content**: Only admin-curated postcards in public gallery
- **Categorization**: Must have valid category and condition rating
- **Image Requirements**: High-quality images with proper attribution

## Domain-Specific Rules

### Vintage Radio Business Logic

#### Business Process
```typescript
interface RadioTransaction {
  step: 'listing' | 'viewing' | 'inquiry' | 'negotiation' | 'purchase' | 'completion';
  requiredFields: string[];
  allowedTransitions: Record<string, string[]>;
}

const radioTransactionRules = {
  transitions: {
    'listing': ['viewing', 'inquiry'],
    'viewing': ['inquiry', 'no_interest'],
    'inquiry': ['negotiation', 'no_interest'],
    'negotiation': ['purchase', 'no_interest'],
    'purchase': ['completion'],
    'completion': ['archived']
  },
  
  requiredFields: {
    'listing': ['name', 'brand', 'model', 'year', 'condition', 'price', 'images'],
    'inquiry': ['message', 'contact_info'],
    'negotiation': ['offer_amount', 'terms'],
    'purchase': ['final_price', 'payment_method', 'shipping_address']
  }
};
```

#### Validation Logic
```typescript
const validateRadioTransaction = (transaction: RadioTransaction): ValidationResult => {
  const errors: string[] = [];
  
  // Check required fields
  const required = radioTransactionRules.requiredFields[transaction.step] || [];
  required.forEach(field => {
    if (!transaction[field]) {
      errors.push(`${field} is required for ${transaction.step} step`);
    }
  });
  
  // Check valid transitions
  const currentStep = transaction.step;
  const allowedTransitions = radioTransactionRules.transitions[currentStep] || [];
  if (transaction.nextStep && !allowedTransitions.includes(transaction.nextStep)) {
    errors.push(`Invalid transition from ${currentStep} to ${transaction.nextStep}`);
  }
  
  return {
    isValid: errors.length === 0,
    errors
  };
};
```

### Pricing and Valuation Rules

#### Pricing Rules
```typescript
interface PricingRule {
  basePrice: number;
  conditionMultiplier: Record<RadioCondition, number>;
  rarityMultiplier: Record<RarityLevel, number>;
  yearMultiplier: number;
  minimumPrice: number;
}

const calculateRadioPrice = (radio: VintageRadio, rule: PricingRule): number => {
  let price = rule.basePrice;
  
  // Apply condition multiplier
  price *= rule.conditionMultiplier[radio.condition];
  
  // Apply rarity multiplier
  price *= rule.rarityMultiplier[radio.rarity];
  
  // Apply year multiplier (older = more valuable)
  const age = new Date().getFullYear() - radio.yearManufactured;
  price *= Math.pow(rule.yearMultiplier, age);
  
  // Ensure minimum price
  price = Math.max(price, rule.minimumPrice);
  
  return Math.round(price * 100) / 100; // Round to 2 decimal places
};
```

#### Commission Rules
```typescript
interface CommissionRule {
  baseCommission: number;
  vatRate: number;
  minimumAmount: number;
  maximumAmount: number;
  sellerDiscount: number; // For verified sellers
}

const calculateCommission = (rule: CommissionRule, salePrice: number, sellerTier: string): number => {
  let commission = (rule.baseCommission / 100) * salePrice;
  
  // Apply seller discount
  if (sellerTier === 'verified') {
    commission *= (1 - rule.sellerDiscount / 100);
  }
  
  // Apply VAT
  const vat = (rule.vatRate / 100) * commission;
  commission += vat;
  
  // Ensure within bounds
  commission = Math.max(commission, rule.minimumAmount);
  commission = Math.min(commission, rule.maximumAmount);
  
  return Math.round(commission * 100) / 100;
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

#### E-commerce Regulations
- **Consumer Rights**: 14-day return policy for online purchases
- **Price Transparency**: All fees and taxes clearly displayed
- **Payment Security**: PCI DSS compliance for payment processing
- **Shipping Information**: Clear delivery times and costs

#### Content Moderation
- **Blog Content**: Fact-checking and accuracy requirements
- **User Reviews**: Moderation for inappropriate content
- **Image Rights**: Proper attribution and copyright compliance
- **Historical Accuracy**: Factual verification for historical content

## Workflow Rules

### Approval Workflows

#### Blog Post Approval
```typescript
interface BlogApprovalWorkflow {
  levels: ApprovalLevel[];
  currentLevel: number;
  status: 'draft' | 'review' | 'approved' | 'rejected';
}

interface ApprovalLevel {
  level: number;
  approverRole: string;
  required: boolean;
  autoApprove: boolean;
}

const processBlogApproval = (workflow: BlogApprovalWorkflow, decision: 'approve' | 'reject'): BlogApprovalWorkflow => {
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

class PricingError extends Error {
  constructor(
    message: string,
    public radioId: string,
    public pricingContext: Record<string, any>
  ) {
    super(message);
    this.name = 'PricingError';
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
  } else if (error instanceof PricingError) {
    // Handle pricing errors
    logger.error('Pricing calculation failed', {
      radioId: error.radioId,
      context: error.pricingContext
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
ALTER TABLE vintage_radios ADD CONSTRAINT positive_price CHECK (price > 0);

-- Example: Ensure valid years
ALTER TABLE vintage_radios ADD CONSTRAINT valid_year CHECK (year_manufactured >= 1900 AND year_manufactured <= EXTRACT(YEAR FROM NOW()));

-- Example: Ensure unique radio identifiers
ALTER TABLE vintage_radios ADD CONSTRAINT unique_radio_identifier UNIQUE (brand, model, year_manufactured, serial_number);

-- Example: Ensure valid condition ratings
ALTER TABLE vintage_radios ADD CONSTRAINT valid_condition CHECK (condition IN ('MINT', 'EXCELLENT', 'GOOD', 'FAIR', 'POOR'));
```

#### Application-Level Validation
```typescript
const validateBusinessRules = (data: any): ValidationResult => {
  const errors: string[] = [];
  
  // Apply business rules
  if (data.price && data.price <= 0) {
    errors.push('Price must be positive');
  }
  
  if (data.yearManufactured && (data.yearManufactured < 1900 || data.yearManufactured > new Date().getFullYear())) {
    errors.push('Year must be between 1900 and current year');
  }
  
  if (data.condition && !['MINT', 'EXCELLENT', 'GOOD', 'FAIR', 'POOR'].includes(data.condition)) {
    errors.push('Invalid condition rating');
  }
  
  // Apply domain-specific rules
  if (data.condition === 'MINT' && data.price < 1000) {
    errors.push('Mint condition radios must cost at least $1,000');
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