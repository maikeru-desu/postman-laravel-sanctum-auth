# 🛠️ Using Postman with Laravel Sanctum SPA Authentication

This guide helps you configure Postman to test Laravel Sanctum's SPA authentication setup.

---

## ✅ 1. Import the Collection

Import your Postman collection to get started.  
![Collection Screenshot](https://maikeru-desu.quest/sanctum-postman/1_create_collection.png)

---

## 🌐 2. Set Up Your Environment

Create a new environment (e.g., `Local`) and define the following variables:

```
base_url = http://localhost:8000
frontend_url = http://localhost:5173
```

![Environment Screenshot](https://maikeru-desu.quest/sanctum-postman/2_create_environments.png)

---

## 📁 3. Organize Routes

Create a folder inside your collection and name it something like `Authentication` (or any name you prefer).  
![Folder Screenshot](https://maikeru-desu.quest/sanctum-postman/3_authentication_folder.png)

---

## 🔐 4. Add CSRF and Login Routes

Inside your `Authentication` folder, create two requests:

- `GET` request to `/sanctum/csrf-cookie`
- `POST` request to `/login` with appropriate credentials in the body

![Routes Screenshot](https://maikeru-desu.quest/sanctum-postman/4_add_csrf_cookie_route.png)
![Routes Screenshot](https://maikeru-desu.quest/sanctum-postman/4_add_login_route.png)

---

## ⚙️ 5. Configure Pre-request Script

Click on the `Authentication` folder → go to the **Pre-request Script** tab → paste the following script:

```js
const jar = pm.cookies.jar();

jar.get(pm.environment.get('frontend_url'), "XSRF-TOKEN",  (err, cookie) => {
    pm.request.addHeader({
        key: "X-XSRF-TOKEN",
        value: cookie
    });

    pm.request.addHeader({
        key: "Referer",
        value: pm.environment.get('frontend_url')
    });
});
```

### 💡 What this script does:

- Fetches the `XSRF-TOKEN` cookie from the `frontend_url`.
- Adds it as a header (`X-XSRF-TOKEN`) to your request.
- Sets the `Referer` header — both are required for Sanctum to validate the request.

![Pre-request Script Screenshot](https://maikeru-desu.quest/sanctum-postman/5_add_script_pre_request.png)

---

## ▶️ 6. Run Auth Flow

1. Run the `GET /sanctum/csrf-cookie` request first.
2. Then, run the `POST /login` request with user credentials.

If successful, Sanctum will authenticate the session and set the auth cookies.

---

## 🔁 7. Apply Script to All Protected Routes

For every folder that contains protected routes (routes that require Sanctum auth), make sure to add the **same Pre-request Script** to the folder.

This ensures that every request gets the correct CSRF token and headers automatically.

---

## ✅ Done!

You're now ready to test Laravel Sanctum SPA authentication using Postman like a frontend SPA client.
