```
@UseInterceptors(ClassSerializerInterceptor)

  @UseFilters(HttpExceptionFilter)

  @Get('id/:id')

  getById(@Param('id', ParseIntPipe) id: number) {

    const user = this.userService.getUserById(id);

    if (user) return new SerializedUser(user);

    else {

      throw new UserNotFoundException('User Was Not Found');

    }

  }
```

```
import { HttpException, HttpStatus } from '@nestjs/common';

  

export class UserNotFoundException extends HttpException {

  constructor(msg?: string, status?: HttpStatus) {

    super(msg || 'User Not Found', status || HttpStatus.BAD_REQUEST);

  }

}
```