markdown
# 🛒 Live Bazar API

**Marketplace Backend System — Django REST Framework**

---

## 1. Overview

**Live Bazar** API bozordagi sellerlar va buyerlar o‘rtasida mahsulot almashinuvi va savdo qilish uchun mo‘ljallangan.  

**Maqsad:**  
- Sellerlar mahsulot qo‘yadi.  
- Buyerlar mahsulotlarni narx, kategoriya va lokatsiya bo‘yicha solishtiradi.  
- Buyerlar basket orqali sotib olishadi.  

**Asosiy Entitylar:**  
- **Seller** — mahsulot qo‘yuvchi foydalanuvchi  
- **Buyer** — xaridor  
- **Product** — sotiladigan mahsulot  
- **Basket** — xaridorning savatchasi  

**Mahsulotlar kategoriyalarga bo‘linadi**, masalan: Elektronika, Avtomobil qismlari, Uy-jihozlari.

---

## 2. API Structure

/api/ ├── sellers/ # Sellerlar CRUD ├── buyers/ # Buyerlar CRUD ├── products/ # Mahsulotlar CRUD va filtrlash ├── basket/ # Savatcha qo‘shish, ko‘rish va boshqarish

Code

**Roles:**  
- **Seller**: mahsulot qo‘yish, tahrirlash, o‘chirish  
- **Buyer**: mahsulotlarni ko‘rish, basketga qo‘shish, sotib olish  

---

## 3. Endpoints Format

Har endpoint `GET /api/{resource}/{id}/` ko‘rinishida bo‘ladi.  
Har bir endpoint uchun:  

- **Path**  
- **Method**  
- **Example Request**  
- **Example Response** (JSON)  

---

## 4. Examples

### 4.1 Seller Qo‘shish

**Path:** `/api/sellers/`  
**Method:** `POST`  

**Request**
```json
{
  "name": "TechnoShop",
  "email": "techno@example.com",
  "phone": "+998901234567",
  "location": "Tashkent"
}
Response (201 Created)

json
{
  "id": 1,
  "name": "TechnoShop",
  "email": "techno@example.com",
  "phone": "+998901234567",
  "location": "Tashkent",
  "created_at": "2025-12-02T11:00:00Z"
}
4.2 Buyer Qo‘shish
Path: /api/buyers/ Method: POST

Request

json
{
  "name": "Jahongir",
  "email": "jahongir@example.com",
  "phone": "+998901112233",
  "location": "Tashkent"
}
Response (201 Created)

json
{
  "id": 1,
  "name": "Jahongir",
  "email": "jahongir@example.com",
  "phone": "+998901112233",
  "location": "Tashkent",
  "created_at": "2025-12-02T11:05:00Z"
}
4.3 Product Qo‘yish (Seller Only)
Path: /api/products/ Method: POST

Request

json
{
  "name": "Apple iPhone 15",
  "price": 1200,
  "category": "Electronics",
  "seller": 1,
  "location": "Tashkent",
  "stock": 10,
  "description": "Latest Apple iPhone model"
}
Response (201 Created)

json
{
  "id": 1,
  "name": "Apple iPhone 15",
  "price": 1200,
  "category": "Electronics",
  "seller": "TechnoShop",
  "location": "Tashkent",
  "stock": 10,
  "description": "Latest Apple iPhone model",
  "created_at": "2025-12-02T11:10:00Z"
}
4.4 Productni Olish
Path: /api/products/{id}/ Method: GET

Example Request

Code
GET /api/products/1/
Response (200 OK)

json
{
  "id": 1,
  "name": "Apple iPhone 15",
  "price": 1200,
  "category": "Electronics",
  "seller": "TechnoShop",
  "location": "Tashkent",
  "stock": 10,
  "description": "Latest Apple iPhone model",
  "created_at": "2025-12-02T11:10:00Z"
}
4.5 Basketga Qo‘shish (Buyer Only)
Path: /api/basket/ Method: POST

Request

json
{
  "buyer": 1,
  "product": 1,
  "quantity": 2
}
Response (201 Created)

json
{
  "id": 1,
  "buyer": "Jahongir",
  "product": "Apple iPhone 15",
  "quantity": 2,
  "total_price": 2400,
  "added_at": "2025-12-02T11:15:00Z"
}
4.6 Basketni Olish
Path: /api/basket/{id}/ Method: GET

Example Request

Code
GET /api/basket/1/
Response (200 OK)

json
{
  "id": 1,
  "buyer": "Jahongir",
  "items": [
    {
      "product": "Apple iPhone 15",
      "price": 1200,
      "quantity": 2,
      "total_price": 2400
    }
  ],
  "basket_total": 2400
}
5. Response Examples (Product)
Har bir mahsulot uchun JSON format:

json
{
  "id": 1,
  "name": "Apple iPhone 15",
  "price": 1200,
  "category": "Electronics",
  "seller": "TechnoShop",
  "location": "Tashkent"
}
🔍 Filtering & Search
Products Filtering:

category — kategoriya bo‘yicha

min_price / max_price — narx oralig‘i

location — shahar bo‘yicha

search — mahsulot nomi bo‘yicha

Example

Code
GET /api/products/?category=Electronics&min_price=1000&location=Tashkent
