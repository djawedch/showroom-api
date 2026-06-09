# Database Design

## Business Logic

1. 3 roles: owner, admin, customer
2. Tables: users, vehicles, categories, favorites, inquiries
3. Vehicle types handled via categories (car, truck, motorcycle, etc.)
4. Public + customer: browse, filter by category/brand/year/price/fuel type, search by brand or model, pagination, view single vehicle with images
5. Customer only: save favorites, submit inquiry on a vehicle
6. Admin: create vehicle, update vehicle, mark as sold (not delete), view listings
7. Admin dashboard: vehicles he listed, vehicles he marked sold, total revenue from his sales
8. Owner: everything admin can do + create/deactivate admin accounts
9. Owner dashboard: total vehicles, total sold, total profit, per-admin performance, stats by month
10. Soft deletes on vehicles
11. Profit tracking: buy_price and sell_price columns on vehicle

## Main Tables

1. **users** (id, first_name, last_name, email, password, phone, role, is_active, created_at, updated_at)
2. **categories** (id, name, slug, created_at, updated_at)
3. **vehicles** (id, category_id, created_by, sold_by, brand, model, year, mileage, fuel_type, transmission, color, buy_price, price, sell_price, description, status, sold_at, created_at, updated_at, deleted_at)
4. **favorites** (id, user_id, vehicle_id, created_at)
5. **inquiries** (id, user_id, vehicle_id, message, status, created_at, updated_at)

### Clarifications

- role(users) : owner, admin, customer
- is_active(users) : owner can deactivate admins
- name(categories) : car, truck, motorcycle
- created_by(vehicles) : which admin created it
- sold_by(vehicles) : nullable - which admin marked it sold
- buy_price(vehicles) : hidden from public - profit tracking
- sell_price(vehicles) : hidden from public - profit tracking
- price(vehicles) : visible to public
- status(vehicles) : available, sold
- sold_at(vehicles) : nullable
- deleted_at(vehicles) : nullable - soft delete
- user_id(favorites) : customer only
- (user_id, vehicle_id) : a unique constraint - a customer can't favorite the same vehicle twice
- status(inquiries) : new, read, replied

## Relationships

1. categories -> vehicles [one-to-many]
2. users -> vehicles [one-to-many] via created_by
3. users -> vehicles [one-to-many] via sold_by
4. users -> favorites [one-to-many]
5. vehicles -> favorites [one-to-many]
6. users -> inquiries [one-to-many]
7. vehicles -> inquiries [one-to-many]
8. vehicles -> media [one-to-many] via spatie