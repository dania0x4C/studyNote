[[API]]

구조

app.put('/api/courses/:id', (req, res) => { 

	// Look up the course 
	// If not existing, return 404
	
	// Validate 
	// If invalid, return 400 - Bad request
	
	// Update course 
	// Return the updated course 
});

```
app.put('/api/courses/:id', (req, res) => { 
	const course = courses.find(c => c.id===parseInt(req.params.id))
	if(!course) res.status(404).send('The course with the given ID was not found');

	const { error } = validateCourse(req.body); //result.error
	if(error){ 
		res.status(400).send(error.details[0].message); 
		return; 
	}

	course.name = req.body.name; res.send(course); 
});

function validateCourse(course){ 
	const schema = { 
		name: Joi.string().min(3).required() 
	}; 
	
	return Joi.validate(course, schema); 
}
```