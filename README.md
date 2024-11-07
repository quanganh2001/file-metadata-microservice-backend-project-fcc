Type `npm i` then `npm i multer`

In your javascript file you would add these lines to access both the file and the body. It is important that you use the `name` field value from the form in your upload function. This tells multer which field on the request it should look for the files in. If these fields aren't the same in the HTML form and on your server, your upload will fail:
```js
const multer = require('multer');
const upload = multer({ dest: './public/data/uploads/' });

app.post('/api/fileanalyse', upload.single('upfile'), function (req, res) {
   // req.file is the name of your file in the form above, here 'uploaded_file'
   // req.body will hold the text fields, if there were any 
   res.json({
    name: req.file.originalname,
    type: req.file.mimetype,
    size: req.file.size
   });
});
```
Full source code:
```js
var express = require('express');
var cors = require('cors');
require('dotenv').config()
const multer = require('multer');
const upload = multer({ dest: './public/data/uploads/' });

var app = express();

app.use(cors());
app.use('/public', express.static(process.cwd() + '/public'));

app.get('/', function (req, res) {
  res.sendFile(process.cwd() + '/views/index.html');
});

app.post('/api/fileanalyse', upload.single('upfile'), function (req, res) {
   // req.file is the name of your file in the form above, here 'uploaded_file'
   // req.body will hold the text fields, if there were any 
   res.json({
    name: req.file.originalname,
    type: req.file.mimetype,
    size: req.file.size
   });
});

const port = process.env.PORT || 3000;
app.listen(port, function () {
  console.log('Your app is listening on port ' + port)
});
```