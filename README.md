# postman
postman scripts



const moment = require("moment")
var current_date_moment = moment().utc().format();
console.log("current date and time in utc", current_date_moment)

if(current_date_moment > (pm.environment.get("access_token_expiry")))
{
const loginRequest = {
    url: 'https://dm-us.informaticacloud.com/authz-service/oauth/token?grant_type=client_credentials',
    method: 'POST',
    header: 'Authorization: '+ pm.environment.get("iics_token")
};

pm.sendRequest(loginRequest, function (err, response) {
    var responseJson = response.json();
    var changed_date_moment = moment().add('seconds', responseJson.expires_in).utc().format()

    pm.environment.set('access_token_expiry', changed_date_moment);
    pm.environment.set("access_token", responseJson.access_token);
    console.log("new access token will expire on", changed_date_moment )
});
}

else{
    console.log("access_token is not expired using the existing token")
}
