[[API]]

npm i joi: validation 검사를 간편하게 해주는 라이브러리

```
app.post("/api/buses/", (req, res) => {

  const schema = {

    name: Joi.string().max(6).required(),

  };

  

  const result = Joi.ValidationError(req.body, schema);

  console.log(result);

  

  if (!req.body.name || req.body.name.length > 6) {

    //validation logic

    res.status(400).send("name is required and should be maximum 5");

  }

  const bus = {

    id: buses.length + 1,

    name: req.body.name,

  };

  buses.push(bus);

  res.send(bus);

});
```