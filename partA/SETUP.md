\# Setup — DummyJSON API



\## Сонгосон API

\*\*DummyJSON\*\* — https://dummyjson.com



\## Brief (тайлбар)

DummyJSON бол үнэгүй placeholder REST API бөгөөд бодит CRUD үйлдлийг 

дэмжсэн fake backend юм. Frontend-ийн прототипжүүлэлт болон API testing-д 

зориулагдсан. Products, Users, Posts, Carts, Auth, Todos, Quotes зэрэг 

олон resource-той.



\## Base URL

`https://dummyjson.com`



\## Authentication

\- Public endpoint-ууд (`/products`, `/users`, `/posts` гэх мэт) auth шаардахгүй

\- `/auth/\*` endpoint-ууд JWT Bearer token шаардана

\- Login: `POST /auth/login` → response-д `accessToken` буцаана

\- Authorization header: `Bearer <accessToken>`

\- Test credentials:

&#x20; - username: `emilys`

&#x20; - password: `emilyspass`



\## Rate limit

Албан ёсны rate limit байхгүй (тестлэхэд тохиромжтой)



\## Документ

https://dummyjson.com/docs



\## Сонгосон шалтгаан

1\. Бүх CRUD үйлдлийг дэмждэг (GET, POST, PUT, DELETE)

2\. JWT login байгаа учир \*\*chained request\*\* дасгал хийх боломжтой

3\. 404 буцаах endpoint бэлэн (`/products/9999`) — negative test

4\. Документац сайн, олон жишээтэй

5\. Rate limit байхгүй учир CI-д улаан болохгүй

