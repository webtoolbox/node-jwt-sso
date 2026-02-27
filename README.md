# node-jwt-sso
Example code for JWT SSO with Node.js for your Website Toolbox forum.

## Usage

Install the dependency:

```bash
npm install jsonwebtoken
```

```javascript
const jwt = require('jsonwebtoken');

const CLIENT_SECRET = 'YOUR_CLIENT_SECRET'; // From Integrate > Single Sign On
const CLIENT_ISSUER_ID = 'YOUR_CLIENT_ISSUER_ID'; // From Integrate > Single Sign On
const FORUM_DOMAIN = 'YOUR_FORUM_DOMAIN'; // e.g. forum.example.com

function createSsoUrl(user) {
    const token = jwt.sign(
        {
            email: user.email,    // Required
            username: user.username, // Optional
            name: user.name,     // Optional
        },
        CLIENT_SECRET,
        {
            algorithm: 'HS256',
            issuer: CLIENT_ISSUER_ID,
            expiresIn: '5m', // Short expiry — only needs to survive the redirect
        }
    );

    return `https://${FORUM_DOMAIN}/oauth?action=doOauthCallback&service=JWT&code=${encodeURIComponent(token)}`;
}

// Redirect the logged-in user to the forum:
res.redirect(createSsoUrl({ email: 'jane@example.com', username: 'jane', name: 'Jane Doe' }));
```

For more details, see the [JWT SSO support article](https://www.websitetoolbox.com/support/article/1045).
