## Deplay Server and Client Side
```js
1.Client side deplay
1.go to firebase Build->hoisting all step compleate and firebase deply ar age "npm run build" dibo  (github : n)
2. kono kicu update korle npm run build and firebase deply dibo
2.Server side :
1.npm i -g vercel
2.vercel login
3.vercel --prod
4 add vercel.json file in the code
{
    "version": 2,
    "builds": [
        {
            "src": "./index.js",
            "use": "@vercel/node"
        }
    ],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "/",
            "methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
        }
    ]
}
5. vercel --prod (kono kicu update korle ata dibo)
```
