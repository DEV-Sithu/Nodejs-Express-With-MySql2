# အဆင့် ၁ 
D အောက်မှာ New Folder ဆောက်ထား  mkdir my-express-app CMD  ကိုဖွင့် D:>FolderName> ရအောင်ပြောင်း
```
C:\Users\user>cd..
C:\Users>cd ..
C:\>D:
D:\>cd \foldername
```

# အဆင့် ၂
Use ‘npm init -y’ for default initialization
> npm init (npm initialization လုပ်လို့ပြောတာ
အဲ့တာကို yes လား no လားမေးတာမို့ -y လို့ထဲ့ပေးတာ)

# အဆင့် ၃
express install လုပ်မယ်  npm install express ြပီးရင် code လို့ရိုက် vscode ကိုတန်းရောက်မယ် 

> Express Server က Http Server code တွေရေးကတာထက်ပိုပြီးရိုးရှင်းမယ် lightweight ဖစ်မယ်  middleware တွေ  http method တေွကိုချိတ်ဆက်ပေးပြီး သုံးရလွယ်တဲံ Rest Api library တစ်ခုဖစ်တယ်

```Project Structure
src/
├── config/         # Environment configurations
├── controllers/    # Route controllers
├── routes/         # Route definitions
├── middleware/     # Custom middleware
├── models/         # Database models
├── services/       # Business logic
├── utils/          # Helper functions
├── validations/    # Validation schemas
├── tests/          # Test suites
├── app.js          # Main app entry
└── initialization
```

# အဆင့် ၄
app.js codeရေး 
```const express = require('express');

const app = express();
const PORT = 3000;

app.listen(PORT, (error) =>{
    if(!error)
        console.log("Server is Successfully Running, 
                   and App is listening on port "+ PORT)
    else 
        console.log("Error occurred, server can't start", error);
    }
);
```
- server စရင် run termial မှာ node server.js  Your server is now live at http://localhost:3000.
- server ရပ်ချင်ရင် Ctrl+C

# အဆင့် ၅
လိုတဲ့ library တွေ install မယ် (package.json ထဲမာ မပေါရင် npm install libararyname --save 
1. npm install mysql2 (MySql)
2. npm install nodemon (Server Restart)
3. npm install dotenv  (Enviroment or Flavor)
4. npm install body-parser (Parse incoming request bodies in a middleware)
5. npm install jsonwebtoken (0auth token)
6. npm install bcryptjs (encryption)
7. npm install --save-dev cross-env (NODE_ENV=production)
8. npm i helmet --save (setting HTTP response headers for security practice)
9. npm i express-rate-limit ( to limit repeated requests to public APIs and/or endpoints such as password reset for security practice)
10. npm install cors (ORS-enabled for all origins for security practice)    
```
"dependencies": {
    "bcryptjs": "^5.1.1",
    "body-parser": "^1.20.3",
    "cors": "^2.8.5",
    "dotenv": "^16.4.7",
    "express": "^4.21.2",
    "express-rate-limit": "^7.5.0",
    "helmet": "^8.0.0",
    "jsonwebtoken": "^9.0.2",
    "mysql2": "^3.12.0",
    "nodemon": "^3.1.9"
  },
  "devDependencies": {
    "cross-env": "^7.0.3"
  }
```
#  အဆင့် ၆
Enviroment file (.env) 
> variable တွေ api key တွေ database config တေွွကို တစ်နေရာထဲမှာ သတ်မှတ်ပေးမယ် လုံခြုံမှုရှိစေဖို့ git commit မလုပ်ရဘူး .gitignore မှာဖွတ်ဖို့ထဲ့ရေးထားပါ
- .env.development
- .env.production
- .env.localhost

.gitignore ဖိုင်ဆောက်
```  env
node_modules/*
package-lock.json
.idea/*
```  
ဒီ variable တွေကို api service မှာ ပြန်ပြီး assign လုပ်ရမယ် 
```   
NODE_ENV=localhost
JWT_SECRET=e3b6846750214cf67d35ae5be45750a96a2543dac426c162aa6f04e5ec5f0010
JWT_EXPIRES_IN=1h
APP_PORT=3000
DB_PORT=3306
DB_HOST=localhost
DB_USER =root
DB_PASS=
MYSQL_DB=dbname
```
JWT Token Generator [https://jwtsecret.com/generate]

## package.json ထဲက script မှာ
```  
 "scripts": {
    "start": "nodemon app.js",
    "start:development": "cross-env NODE_ENV=development node app.js",
    "start:localhost": "cross-env NODE_ENV=localhost node app.js",
    "start:production": "cross-env NODE_ENV=production node app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```           
run ချင်ရင် ဘယ်ဟာနဲ့ ရွေးပြီး run မလဲဖစ်မယ် start:localhost လေးကိုcursorချ Run scriptနဲ့ရွေး errorတတ်တာဖစ်ဖစ် server stopချင်ဖစ်ဖစ် delete icon နိုပ်
 node app.js ကို flavor ခွဲပေးထားတာ

# အဆင့် ၇ 
app.js မှာရေး
```   
// dot env variableတွေimportလုပ်တာ
require('dotenv').config();
// app.js ထဲမာဒီjsမှာခသူံးမာလောက်ပဲimport
const express = require('express');
const bodyparser = require('body-parser');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const cors = require('cors');

// express app serverတစ်ခုဆောက်ပီ
const app = express();

// middleware 
app.use(express.json());
app.use(bodyparser.urlencoded({ extended: true }));
app.use(bodyparser.json());
app.use(helmet());
app.use(cors());
app.use(rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
}));

//env ဖိုင်ကွဲတွေထဲကမှ runထားတဲ့nodeရဲ့variable
require('dotenv').config({ path: `.env.${process.env.NODE_ENV}` });

// Route တွေ ရေးရမယ်
// User 
const v1UserRoutes = require('./routes/v1/userRoutes');
const v2UserRoutes = require('./routes/v2/userRoutes');
app.use('/v1', v1UserRoutes);
app.use('/v2', v2UserRoutes);

// Auth
const v1AuthRoutes = require('./routes/v1/authRoutes');
app.use('/v1', v1AuthRoutes);

// version ခွဲတာနဲ့ပတ်သက်ပြီးရေးဝာာ
const versionMiddleware = require('./middleware/versioning');

// လက်ရှိဘယ်versionကိုသုံးနေပီသတ်မှတ်
app.use(versionMiddleware('v1')); // Default to v1

// api ကနေလဲ checkversionနဲ့စစ်ရင်သိရအောင်
app.get('/checkVersion', (req, res) => {
  if (req.version === 'v1') {
    res.send('Response from v1');
  } else if (req.version === 'v2') {
    res.send('Response from v2');
  }
});

const port = process.env.APP_PORT || 5000;

// serverအလုပ်လုပ်မလုပ် စစ်တာ
app.get("/", (req, res) => {
  res.json({ message: "Best Practice Code for Node Js Express with MySql" });
});

// app ကို တောက်လျောက် run ခိုင်းထားတာ
app.listen(port, function (error) {
    if (error) throw error
    console.log("Server created Successfully on PORT", port);
  });

```
# အဆင့် ၈
database.js မှာ mongodb သုံးမှာလား mysql သုံးမှာလားရွေးချယ်နိုင်ပါတယ် လောလောဆယ် mysql2 ကိုသုံးရင်ရေးနည်း
```
const {createPool} = require('mysql2/promise');
require('dotenv').config();

const pool = createPool({
    port: process.env.DB_PORT,
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASS,
    database: process.env.MYSQL_DB,
    connectionLimit: 10,
    waitForConnections: true,
    queueLimit: 0
});

module.exports = pool;
```
# အဆင့် ၉
