# TESTIMONY GRAPHIX - Customer Code System Guide

## Overview

The Customer Code System is a comprehensive solution for managing graphic design clients and their orders. Each customer receives a unique code (e.g., `TGX-A2B3-C4D5`) that serves as their identifier throughout their relationship with TESTIMONY GRAPHIX.

## Customer Code Format

- **Format**: `TGX-XXXX-XXXX`
- **Example**: `TGX-A2B3-C4D5`
- **Components**:
  - `TGX` = TESTIMONY GRAPHIX prefix
  - `XXXX` = 8 random alphanumeric characters

## Key Features

### 1. Customer Registration
Create a new customer account and receive a unique code.

**Required Information**:
- Name
- Email
- Phone (optional)
- Company (optional)
- Communication preference
- Notes (optional)

**Example**:
```json
{
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "+1-555-0100",
  "company": "ABC Corporation",
  "communicationMethod": "email",
  "notes": "Prefers modern, minimalist design"
}
```

### 2. Order Management
Track design projects with detailed status updates.

**Order Statuses**:
- `pending` - Order received, awaiting designer assignment
- `in-progress` - Designer is actively working on the project
- `review` - Completed and awaiting customer review
- `completed` - Client approved, ready for final delivery
- `delivered` - Handed over to customer

**Order Information**:
- Service Type (logo, branding, print, web, etc.)
- Budget
- Due Date
- Deliverables
- Maximum Revisions Allowed (default: 3)
- Revision Tracking

### 3. File Management
Upload and manage design files and deliverables.

**Supported File Types**:
- `jpg`, `png`, `gif` - Raster images
- `ai`, `eps` - Adobe Illustrator
- `psd` - Adobe Photoshop
- `pdf` - Portable Document Format
- `svg` - Vector graphics
- Other design formats

### 4. Revision Tracking
Monitor and manage design revisions.

**Features**:
- Maximum revision limits per order
- Track revision requests with notes
- Automatic revision counter
- Prevent over-revision abuse

### 5. Customer Analytics
View comprehensive customer statistics.

**Metrics**:
- Total orders
- Completed orders
- Total amount invested
- Average order value
- Lifetime customer value

## API Endpoints

### Customer Management

#### Register New Customer
```
POST /api/customers/register
Content-Type: application/json

{
  "name": "Jane Doe",
  "email": "jane@company.com",
  "phone": "+1-555-0101",
  "company": "XYZ Inc",
  "communicationMethod": "email"
}
```

**Response**:
```json
{
  "success": true,
  "customer": {
    "code": "TGX-K9M2-L5P8",
    "name": "Jane Doe",
    "email": "jane@company.com",
    "createdAt": "2024-01-15T10:30:00Z",
    "status": "active",
    "orders": []
  }
}
```

#### Get Customer by Code
```
GET /api/customers/TGX-K9M2-L5P8
```

#### Get Customer by Email
```
GET /api/customers/email/jane@company.com
```

#### Get Customer Summary
```
GET /api/customers/TGX-K9M2-L5P8/summary
```

**Response**:
```json
{
  "success": true,
  "data": {
    "customer": { ... },
    "summary": {
      "totalOrders": 5,
      "completedOrders": 3,
      "totalInvested": 15000,
      "averageOrderValue": 3000
    },
    "orders": [ ... ]
  }
}
```

### Order Management

#### Create Order
```
POST /api/orders
Content-Type: application/json

{
  "customerCode": "TGX-K9M2-L5P8",
  "title": "Company Logo Design",
  "description": "Modern, minimalist logo with brand guidelines",
  "serviceType": "logo",
  "budget": 2500,
  "dueDate": "2024-02-15",
  "deliverables": ["Logo AI file", "PNG variants", "Brand guidelines"],
  "maxRevisions": 3,
  "notes": "Client prefers blue and green colors"
}
```

**Response**:
```json
{
  "success": true,
  "order": {
    "id": "TGX-K9M2-L5P8-1705316400000",
    "customerCode": "TGX-K9M2-L5P8",
    "title": "Company Logo Design",
    "status": "pending",
    "budget": 2500,
    "trackingUrl": "https://testimony-graphix.com/track/TGX-K9M2-L5P8-1705316400000",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

#### Get Customer Orders
```
GET /api/customers/TGX-K9M2-L5P8/orders
```

#### Update Order Status
```
PUT /api/orders/TGX-K9M2-L5P8-1705316400000/status
Content-Type: application/json

{
  "status": "in-progress"
}
```

#### Add File to Order
```
POST /api/orders/TGX-K9M2-L5P8-1705316400000/files
Content-Type: application/json

{
  "name": "Logo_Final.ai",
  "type": "ai",
  "size": 2048576,
  "url": "https://storage.testimony-graphix.com/files/logo-final.ai",
  "description": "Final logo in Adobe Illustrator format"
}
```

#### Request Revision
```
POST /api/orders/TGX-K9M2-L5P8-1705316400000/revision
Content-Type: application/json

{
  "revisionNotes": "Change the blue to darker shade, increase size of text"
}
```

## Workflow Example

### Step 1: Customer Registration
1. Prospect visits website or contacts via email
2. Customer info is collected
3. Unique code generated: `TGX-A2B3-C4D5`
4. Code sent to customer via email

### Step 2: Order Creation
1. Customer submits project requirements
2. Order created under customer code
3. Order assigned to designer
4. Status set to `pending`

### Step 3: Design Phase
1. Designer begins work
2. Status changed to `in-progress`
3. Designer uploads draft files
4. Customer notified of progress

### Step 4: Review & Revision
1. Status changed to `review`
2. Customer downloads and reviews files
3. If changes needed, revision requested
4. Designer makes changes
5. Revision counter incremented
6. Files re-uploaded

### Step 5: Completion & Delivery
1. Once approved, status set to `completed`
2. Final files uploaded
3. Status changed to `delivered`
4. Customer receives delivery notification
5. Order marked complete

## Best Practices

### For Designers
- ✅ Keep customers updated on status changes
- ✅ Respond promptly to revision requests
- ✅ Upload files with clear descriptions
- ✅ Document design decisions and rationale
- ✅ Use revision notes to guide next iteration

### For Customers
- ✅ Provide detailed project requirements upfront
- ✅ Use revision requests to communicate clearly
- ✅ Respect revision limits
- ✅ Download and archive your files
- ✅ Provide feedback within reasonable timeframes

### For Managers
- ✅ Monitor orders by status and due date
- ✅ Track customer lifetime value
- ✅ Identify top customers for special treatment
- ✅ Use analytics to improve processes
- ✅ Regular follow-up with inactive customers

## Integration Examples

### Database Storage
Customer data can be stored in any database system:
```javascript
const customer = customerSystem.registerCustomer(data);
// Export to JSON
const jsonData = customerSystem.exportCustomerData(code);
// Save to database
await db.customers.insert(JSON.parse(jsonData));
```

### Email Notifications
Trigger notifications on status changes:
```javascript
customerSystem.updateOrderStatus(orderId, 'completed');
// Automatically send: "Your order is complete and ready for download"
// Email: customer.email
```

### CRM Integration
Export customer data to CRM systems:
```javascript
const summary = customerSystem.generateCustomerSummary(code);
// Push to Salesforce, HubSpot, Pipedrive, etc.
```

## Security Considerations

1. **Code Privacy**: Customer codes should be treated as sensitive information
2. **Email Verification**: Verify email addresses on registration
3. **Access Control**: Only customers can view their own order details
4. **File Security**: Store files securely and limit access
5. **Audit Trail**: Log all status changes and file uploads

## Future Enhancements

- [ ] Payment integration (Stripe, PayPal)
- [ ] Automated invoice generation
- [ ] Calendar integration for due dates
- [ ] Feedback/review system
- [ ] Team collaboration tools
- [ ] Portfolio showcase from completed orders
- [ ] Analytics dashboard
- [ ] Email notification automation
- [ ] File version control
- [ ] Time tracking integration

## Support

For questions or issues, contact: support@testimony-graphix.com
