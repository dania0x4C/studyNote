
```
const request = require('request');

  

var url = 'http://apis.data.go.kr/1613000/BusLcInfoInqireService/getRouteAcctoBusLcList';

var queryParams = '?' + encodeURIComponent('serviceKey') + '=rpF5tThUUqO49KRisZSu0oazJROB6fq7lzmgJq6JBEBbqNpXd1LycCoJOy5XerfyyWsxYg2d97%2FZwriVHTc3zQ%3D%3D'; /* Service Key*/

queryParams += '&' + encodeURIComponent('pageNo') + '=' + encodeURIComponent('1'); /* */

queryParams += '&' + encodeURIComponent('numOfRows') + '=' + encodeURIComponent('10'); /* */

queryParams += '&' + encodeURIComponent('_type') + '=' + encodeURIComponent('json'); /* */

queryParams += '&' + encodeURIComponent('cityCode') + '=' + encodeURIComponent('25'); /* */

queryParams += '&' + encodeURIComponent('routeId') + '=' + encodeURIComponent('DJB30300052'); /* */

  

request({

    url: url + queryParams,

    method: 'GET'

}, function (error, response, body) {

    //console.log('Status', response.statusCode);

    //console.log('Headers', JSON.stringify(response.headers));

    //console.log('Reponse received',body );

    const jsonResponse = JSON.parse(body);

    const routeType = jsonResponse.response.body.items.item[0].routetp;

    console.log('Route Type:', routeType);

});
```