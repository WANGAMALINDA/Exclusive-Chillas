# Supabase Database Setup Guide

Your project now uses Supabase PostgreSQL for data persistence. Follow these steps to create the required database tables:

## Connection Details
- **Project URL:** https://skugimdlhhxoefptovcl.supabase.co
- **Database:** postgres
- **Host:** db.skugimdlhhxoefptovcl.supabase.co

## Steps

### 1. Access Supabase SQL Editor
1. Go to https://app.supabase.com
2. Select your project "Exclusive Chillas"
3. Navigate to **SQL Editor** (left sidebar)
4. Click **New Query**

### 2. Create Orders Table
Copy and paste this SQL, then click **Run**:

```sql
CREATE TABLE orders (
  id BIGSERIAL PRIMARY KEY,
  customer_name TEXT,
  customer_phone TEXT,
  items_json TEXT,
  total NUMERIC,
  status TEXT DEFAULT 'new',
  order_type TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 3. Create Bookings Table
Copy and paste this SQL, then click **Run**:

```sql
CREATE TABLE bookings (
  id BIGSERIAL PRIMARY KEY,
  fname TEXT,
  lname TEXT,
  phone TEXT,
  guests INTEGER,
  pkg TEXT,
  event_date DATE,
  event_type TEXT,
  notes TEXT,
  status TEXT DEFAULT 'new',
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 4. Create Enquiries Table (Optional)
Copy and paste this SQL, then click **Run**:

```sql
CREATE TABLE enquiries (
  id BIGSERIAL PRIMARY KEY,
  customer_name TEXT,
  customer_phone TEXT,
  enquiry_type TEXT,
  message TEXT,
  status TEXT DEFAULT 'new',
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 5. Enable Row Level Security (Optional but Recommended)
For each table:
1. Go to **Authentication** → **Policies**
2. Select each table
3. Click **Enable RLS**
4. Add a policy allowing public read/write:
   ```sql
   CREATE POLICY "Enable read and write for all" ON orders
   FOR ALL USING (true) WITH CHECK (true);
   ```

## Testing

After creating the tables:

1. **Test from Customer Site:** Open `index.html` in browser
   - Place a food order
   - Check if it appears in Supabase **Table Editor** → `orders` table

2. **Test from POS Dashboard:** Open `pos.html`
   - Should automatically load orders from database
   - Create a booking to test persistence
   - Change order status and refresh page to verify it persists

## Troubleshooting

If data doesn't appear:
- Check browser console (F12) for errors
- Verify table names match exactly (lowercase, no spaces)
- Confirm Supabase credentials in code match your project
- Check that RLS policies allow public access if enabled

## API Structure

The code uses Supabase REST API with this format:
- **GET:** `{URL}/rest/v1/{table}?{filters}`
- **POST:** `{URL}/rest/v1/{table}` (insert)
- **PATCH:** `{URL}/rest/v1/{table}?id=eq.{id}` (update)

All requests use the publishable key in headers:
```
apikey: sb_publishable_es--Qz0KW3aFw8Cckgqf9g_x7iW4yQQ
Authorization: Bearer {key}
```
