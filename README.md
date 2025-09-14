# ReqRes API Postman Project

# ReqRes API Testing with Postman + Newman

This project demonstrates API testing using [Postman](https://www.postman.com/) and [Newman](https://github.com/postmanlabs/newman).

## 📂 Project Structure

reqres-postman/
├─ collections/ # Postman collection JSON (exported)
├─ environments/ # Postman environment JSON (exported)
├─ reports/ # Newman HTML reports (generated)
├─ package.json # Node.js project config
└─ README.md # Project documentation


---

## 🧪 Test Scenarios

- **Auth**
  - `POST /login` (valid login)
  - `POST /login` (invalid login → missing password)

- **Users**
  - `GET /users?page=2`
  - `POST /users` (create new user)
  - `PUT /users/{id}` (update user)
  - `DELETE /users/{id}` (delete user)

- **Negative**
  - Invalid endpoint
  - Invalid payloads

---

## ⚙️ Setup Instructions

1. **Clone this repository**
   ```bash
   git clone <your-repo-url>
   cd reqres-postman
   
2. Install dependencies
	npm install
	
3. Export collection & environment from Postman

• Save the exported files into:

	• collections/ReqRes.postman_collection.json
	• environments/ReqRes.postman_environment.json
	
▶️ How to Run Tests

Run the collection with Newman:

npx newman run ./collections/ReqRes.postman_collection.json \
  -e ./environments/ReqRes.postman_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export ./reports/reqres-report.html
  
  • Results show in the CLI

  • A detailed HTML report is saved in reports/reqres-report.html
  
 📊 Sample Newman Report

When executed, Newman will generate an interactive HTML report (via https://www.npmjs.com/package/newman-reporter-htmlextra
) showing test outcomes and response details.

💡 Next Steps

	• Add more test cases (edge cases, error handling).
	• Integrate with GitHub Actions (.github/workflows/run-postman.yml) to run automatically on every push.
	• Extend tests with Postman scripting (JS assertions).
	
👨‍💻 Maintainer: Taief Hasan
📧 taief.hasan880@gmail.com

🔗 LinkedIn (https://www.linkedin.com/in/taif-hasan)


---

✅ This version is **professional and GitHub-ready**:  
- Explains the structure  
- Lists test cases  
- Provides exact setup & run steps  
- Leaves room for future CI/CD  





