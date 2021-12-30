# code-redeemer
☢️ WARNING: this is just drafts ☢️

---
**Generate:**
```js
const Code = require("../../database/Schemas/Code");
const Utils = require("../../utils/main.js");

async function createCodes(award, expTime) {
  const now = new Date();
  now.setHours(now.getHours + expTime)
  
  const code = await Code.create({
    code: Utils.generateCodes(),
    award: "coins:2000",
    timeLimit: now
  });
}
```

---
**Schema:**
```js
const { Schema, model, Types } = require("mongoose");

const CodeSchema = new Schema({
  code: { type: String, require: true, unique: true },
  award: { type: String, require: true },
  createdAt: { type: Date, default: Date.now },
  timeLimit: { type: Date, require: true }
})
```

---
**Utils:**
```js
module.exports = {
  function generateCodes() {
    return ((Date.now() * process.uptime()) * Math.floor(Math.random() * 10000)).toString(16).slice(0,9)
  } 
}
```

---
**Rescue:**
```js
const Code = require("../../database/Schemas/Code.js");
const User = require("../../database/Schemas/User.js");
const Utils = require("../../utils/main.js");

function redeemCode(code, userId) {
  const dataCode = await Code.find({ code });
  const user = await User.findById(userId);
  
  if (!dataCode)
    return message.reply("invalid code");
  
  if (Date.now() - dataCode.timeLimit.now() < 0)
    return message.reply("expired code");
    await Code.findByIdAndremove(dataCode._id)
  
  const [typeSet, reward] = data.award.split(":");
  
  switch (typeSet) {
    case "coins": {
      await findByIdAndUpdate(
        userId,
        { coins: parseInt(reward) }
      );
    }
    // if you have a vip system or boxes, just put it here
  }
}
```
