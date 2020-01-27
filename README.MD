# NodeJS Shopping Cart - NodeJS, Express, Express-Session, Express handlebars (hbs)

### Run locally on your machine
You need to have installed Node and NPM.

![NodeJS Shopping Cart](/data/nodejs-cart-1.png?raw=true "NodeJS Shopping Cart")

Clone this repository
``` shell
git clone https://github.com/JENkt4k/nodejs-shopping-cart.git
```

Install dependencies
``` shell
npm install
```

Start NodeJS server at http://localhost:3000
```shell
docker-compose up --build
```

Start with debugging
```shell
docker-compose -f .\docker-compose.debug.yml  up --build
```


### Technologies
NodeJS, Express, Express-Session, Nodemon, hbs - Express.js view engine for handlebars.js

Original code provided by: gtsopour@gmail.com, this is a for with Docker debugging 

# Debug With Docker!!!

The purpose of this repo was to pull an existing project and attempt to debug. 

"status_patch" contains the changes required to make debugging work:
```diff
diff --git a/Dockerfile b/Dockerfile
new file mode 100644
index 0000000..ce1f9e6
--- /dev/null
+++ b/Dockerfile
@@ -0,0 +1,9 @@
+FROM node
+
+COPY package.json package.json  
+RUN npm install
+
+COPY . .  
+
+EXPOSE 3000
+CMD ["node", "./bin/www"]
\ No newline at end of file
diff --git a/docker-compose.debug.yml b/docker-compose.debug.yml
new file mode 100644
index 0000000..3a4a001
--- /dev/null
+++ b/docker-compose.debug.yml
@@ -0,0 +1,12 @@
+version: '3.6'
+
+services:
+  app: 
+    build: .
+    ports:
+      - "3000:3000" 
+      - "9229:9229"
+    command:
+      - node
+      - "--inspect-brk=0.0.0.0"
+      - "./bin/www"
\ No newline at end of file
diff --git a/docker-compose.yml b/docker-compose.yml
new file mode 100644
index 0000000..15be0b9
--- /dev/null
+++ b/docker-compose.yml
@@ -0,0 +1,7 @@
+version: '3.6'
+
+services:
+  app: 
+    build: .
+    ports:
+      - "3000:3000" 
\ No newline at end of file
diff --git a/notes.txt b/notes.txt
new file mode 100644
index 0000000..d126e19
--- /dev/null
+++ b/notes.txt
@@ -0,0 +1,6 @@
+docker-compose -f .\docker-compose.debug.yml  up --build
+
+
+package.json
+-- scripts
+ ->  "dev": "node --inspect=0.0.0.0:9229 ./app.js"
\ No newline at end of file
diff --git a/package.json b/package.json
index f6afc2e..5b39ab3 100644
--- a/package.json
+++ b/package.json
@@ -5,7 +5,8 @@
   "email": "gtsopour@gmail.com",
   "private": false,
   "scripts": {
-    "start": "node ./bin/www"
+    "start": "node ./bin/www",
+    "dev": "node --inspect=0.0.0.0:9229 ./bin/www"
   },
   "dependencies": {
     "body-parser": "^1.18.3",

```