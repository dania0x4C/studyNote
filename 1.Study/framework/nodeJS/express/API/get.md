[[API]]

```
app.get("/bus/:bus/:", (req, res) => {

    res.send([req.params.bus, 30, 3]);

});

//params.id(parameter) = :id

app.get("/bus/:bus/:time", (req, res) => {

    res.send(req.params);

});

  

const buses = [

    {id: 1, name: 'bus95'},

    {id: 2, name: 'bus89'},

]

  

app.get('/api/buses/:id', (req, res) => {

    const bus = buses.find(c => c.id === parseInt(req.params.id));

    if(!bus) res.status(404).send('The course with the given ID was not found');

    res.send(bus);

});
```