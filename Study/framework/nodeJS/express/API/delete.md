[[API]]

구조

app.delete('/api/courses/:is', (req, res) => { 

	// Look up the course 
	// Not existing, return 404 
	
	// Delete 
	
	// Return the same course 

});

```
app.delete('/api/courses/:id', (req, res) => { 
	const course = courses.find(c => c.id === parseInt(req.params.id))
	if(!course) return res.status(404).send('The course with the given ID was not found');
	
	const index = courses.indexOf(course); 
	courses.splice(index, 1); 
	
	res.send(course); 
});
```