See the "Airline Management System Final Report.pdf" file for more specific details.
# AMS Rest API Documentation

## Formatting guideline
* Use **2 spaces** for indentation of `json` data
* Use ticks (\`\`) to display `/endpoints`.
* Use triple ticks (\`\`\`json) to display your input and outputs.
* Use \#\# for domains (for example, **Registration, Login, Flight**)
* Use \#\#\# for `endpoints`
* Use \#\#\#\# for **Input** and **Output** sections.

## Login
### `POST /login`
#### Input
```json
// User
{
  "is_employee": false,
  "key": "b@ams.com", // Email for user.
  "password": "123456"
}

// Employee
{
  "is_employee": true,
  "key": "00000000000", // National id for employee.
  "password": "123456"
}
```
#### Output
```json
// If logged in as USER
{
  "token": /* A JSON Web Token */
}
````

## Registration
### `/register`
#### Input
```json
{
  "name": "Berkay",
  "surname": "Dinc",
  "email": "berkayydinc@gmail.com",
  "password": "123456",
  "phone": "+90 543 589 57 98",
  "gender": "Male",
  "birth_date": "2002-08-14", // YYYY-MM-DD
}
```
#### Output
Returns `409` on error, `200` on success.
```json
{
  "token": /* A JSON Web Token */
}
```

## Flight
::::danger
:warning: `/flight` routes doesn't return past or onflight flights, only scheduled. To get past flights, refer to `/flight/all`
::::

### `GET /flight`
### `GET /flight/seats`
### `GET /flight/:flight_number`
### `GET /flight/:from/:to/:date`
::::warning
Requires **Admin**  or **Flight Planner** permission. Otherwise returns **403**.
::::
### `POST /flight/cancel/`
#### Input
```json
{
	"flight_number": "TK3535"
}
```
::::warning
All `/flight/all` routes requires **Admin** or **Flight Planner** permission. Otherwise returns 403.
::::
### `GET /flight/all`
### `GET /flight/all/seats`
### `GET /flight/all/:id`
### `GET /flight/all/:from/:to/:date`

## Passenger
::::info
:information_source: Average passenger output:
```json
{
  "national_id": "12345678901",
  "pnr_no": "ABC12345",
  "flight_number": "TK101",
  "baggage_allowance": 25,
  "luggage_id": "987654321012",
  "fare_type": "Essentials",
  "seat": 1,
  "meal": 1,
  "extra_luggage": 20,
  "check_in": 1,
  "name": "John",
  "surname": "Doe",
  "email": "johndoe@gmail.com",
  "phone": "1234567890",
  "gender": "Male",
  "birth_date": "1980-05-14T21:00:00.000Z",
  "cip_member": 0,
  "vip_member": 0,
  "disabled": 0,
  "child": 0,
  "buyer": 1
}
```
::::
### `GET /passenger/all`
::::warning
Requires **Admin**  or **Passenger Services** permission. Otherwise returns **403**.
::::
#### Output
```json
[
  {
     "national_id": "12345678900",
     "pnr_no": "BCD67891",
     "baggage_allowance": 15,
     "luggage_id": "098765432101",
     "fare_type": "NaN",
     "seat": "NaN",
     "meal": 1,
     "extra_luggage": 15,
     "check_in": 0,
     "name": "Ella",
     "surname": "Moore",
     "email": "ellamoore@gmail.com",
     "phone": "1234567890",
     "gender": "Female",
     "birth_date": "1973-09-01T21:00:00.000Z",
     "cip_member": 0,
     "vip_member": 0,
     "disabled": 0,
     "child": 0
  },
  {
     "national_id": "12345678901",
     "pnr_no": "ABC12345",
     "baggage_allowance": 25,
     "luggage_id": "987654321012",
     "fare_type": "NaN",
     "seat": "NaN",
     "meal": 1,
     "extra_luggage": 20,
     "check_in": 1,
     "name": "John",
     "surname": "Doe",
     "email": "johndoe@gmail.com",
     "phone": "1234567890",
     "gender": "Male",
     "birth_date": "1980-05-14T21:00:00.000Z",
     "cip_member": 0,
     "vip_member": 0,
     "disabled": 0,
     "child": 0
  }
]
```
### `GET /passenger/id`
::::warning
Requires **Admin**  or **Passenger Services** permission. Otherwise returns **403**.
::::
#### Input
`?id=12345678901`
### `GET /passenger/pnr`
::::warning
Requires **Admin**  or **Passenger Services** permission. Otherwise returns **403**.
::::
#### Input
`?pnr=ABC12345&surname=Doe`
### `POST /passenger/emplyeecheckin`
::::warning
Requires **Admin**  or **Passenger Services** permission. Otherwise returns **403**.
::::
#### Input
```json
{
	"pnr" : "45BHSM26",
	"weight" : "23.4",
	"piece" : "2"
}
```
`• Check-in process performed at the counter by passenger services`

### `POST /passenger/checkin`
::::info
Requires no auth.
::::
#### Input
```json
{
	"pnr" : "KLM78958",
  "surname": "Cooper" 
  
}
```
`• Online check in process. Weight and piece are '0' by default and can be updated by employeecheckin`
### `POST /passenger/checkin`
::::info
Requires no auth.
::::
#### Input
```json
{
	"pnr" : "KLM78958",
  "surname": "Cooper" 
}
```
### `GET /passenger/meal`
::::warning
Requires **Admin**, **Passenger Services** or **Ground Services** permission. Otherwise returns **403**.
::::
#### Output
```json
[
	{
		"seat": 0,
		"name": "Ella",
		"surname": "Moore",
		"flight_number": "TK101"
	},
	{
		"seat": 1,
		"name": "John",
		"surname": "Doe",
		"flight_number": "TK101"
	},
	{
		"seat": 5,
		"name": "Lily",
		"surname": "Knight",
		"flight_number": "TK101"
	},
    .
    .
    .
]
```
### `GET /passenger/meal/:flight_number"`
::::warning
Requires **Admin**, **Passenger Services** or **Ground Services** permission. Otherwise returns **403**.
::::
#### Output
```json
[
	{
		"seat": 0,
		"name": "Ella",
		"surname": "Moore"
	},
	{
		"seat": 1,
		"name": "John",
		"surname": "Doe"
	},
	{
		"seat": 5,
		"name": "Lily",
		"surname": "Knight"
	},
    .
    .
    .
]
```
### Getting all distinct persons
### `GET /passenger/:flight_number/:distinctive"`
::::warning
Requires **Admin**, **Passenger Services** or **Ground Services** permission. Otherwise returns **403**.
::::
::::info
:information_source: 
:distinctive = {disabled, cip_member, child}.
::::
#### Input
`http://localhost:5500/passenger/TK4040/cip_member`
#### Output
```json
[
	{
		"national_id": "13429215284",
		"pnr_no": "1SHBNGCB",
		"flight_number": "TK4040",
		"baggage_allowance": 45,
		"luggage_id": "185528654152",
		"fare_type": "comfort",
		"seat": 2,
		"meal": 1,
		"extra_luggage": 0,
		"check_in": 0,
		"name": "Eric",
		"surname": "Johson",
		"email": "ej@gmail.com",
		"phone": "5239654872",
		"gender": "Male",
		"birth_date": "1994-12-31T22:00:00.000Z",
		"cip_member": 1,
		"vip_member": 0,
		"disabled": 0,
		"child": 0,
		"buyer": 4
	},
	{
		"national_id": "19219219234",
		"pnr_no": "45BHSM26",
		"flight_number": "TK4040",
		"baggage_allowance": 45,
		"luggage_id": "687100075239",
		"fare_type": "comfort",
		"seat": 11,
		"meal": 1,
		"extra_luggage": 0,
		"check_in": 1,
		"name": "Güney",
		"surname": "Dinc",
		"email": "guneydinc@gmail.com",
		"phone": "5465368978",
		"gender": "Male",
		"birth_date": "1994-12-31T22:00:00.000Z",
		"cip_member": 1,
		"vip_member": 0,
		"disabled": 0,
		"child": 0,
		"buyer": 4
	},
                 .
                 .
                 .
]
```
### `POST /passenger/remove`
::::info
Requires no auth.
::::
#### Input
```json
{
	"pnr_no": "KLM78958",
	"surname": "Cooper" 
}
```

## Check-in & Payment

### `POST /ticket/payment`
#### Input
```json
{
	"token" : "4",
	"flight_number" : "TK2222",
	"ticket_type" : "comfort",
	"national_id" : "13429215284",
	"seat" : "2",
	"name" : "Eric",	
	"surname" : "Johson",
	"email" : "ej@gmail.com",
	"phone" : "5239654872",
	"gender" : "male",
	"birth_date" : "1995-01-01",
	"disabled" : "0",
	"child" : "0"
}
```
#### Output
```json
{
	"pnr": "'1SHBNGCB'"
}
```

`200` on success.
`404` on error.
###### Error Types
* `Flight Not Found`
* `Not Found`
* `Not Enough Token`
* `Payment Error`
* `Error`
* `Flight Not Found`

## Plane
### Get all planes
### `POST /plane/all`
::::warning
Requires **Admin**  or **Flight Planner** permission. Otherwise returns **403**.
::::
#### Output
```json
{
	[
	{
		"plane_registration": "TC-ABC",
		"model": "Boeing 777",
		"location": "ADB",
		"max_passengers": 270,
		"is_active": 1
	},
	{
		"plane_registration": "TC-ACB",
		"model": "Airbus A350",
		"location": "ADB",
		"max_passengers": 270,
		"is_active": 1
	},
	{
		"plane_registration": "TC-ANA",
		"model": "Airbus A350",
		"location": "ADB",
		"max_passengers": 270,
		"is_active": 1
	},
	{
		"plane_registration": "TC-BBC",
		"model": "Airbus A350",
		"location": "ADB",
		"max_passengers": 270,
		"is_active": 1
	},
	
                           .
                           .
                           .
	
}
```

### Get plane by registration

### `GET  /plane/:registration`
::::warning
Requires **Admin**  or **Flight Planner** permission. Otherwise returns **403**.
::::

::::info
Example: ` http://localhost:5500/plane/TC-ABC`
::::

#### Output
```json
{
	"plane_registration": "TC-ABC",
	"model": "Boeing 777",
	"location": "ADB",
	"max_passengers": 270,
	"is_active": 1
}
```  
### Add Plane

### `POST  /plane/add`

::::warning
Requires **Admin** permission. Otherwise returns **403**.
::::

#### Input
```json
{
	"registration" : "TC-EZG",
	"model" : "Airbus A350"
}
```    
#### Output
`Succes : 200 , Insertion Error : 404 `

### Change Plane Status

### `POST  /plane/status`

::::warning
Requires **Admin** permission. Otherwise returns **403**.
::::

#### Input
```json
{
	"registration" : "TC-EZG",
	"status" : "1"
}
```    
#### Output
`Succes : 200 , Insertion Error : 404 `

## Profile
### Get Profiles

### `GET /profile/users`
::::warning
Requires **Admin** permission. Otherwise returns **403**.
::::

#### Output
```json
[
	{
		"id": 1,
		"name": "Yusuf",
		"surname": "Gassaloglu",
		"email": "y@ams.com",
		"password": "123456",
		"phone": "05360000222",
		"gender": "Male",
		"birth_date": "2001-03-12T22:00:00.000Z",
		"money": 700715
	},
	{
		"id": 4,
		"name": "Berkay",
		"surname": "Dinc",
		"email": "b@ams.com",
		"password": "123456",
		"phone": "05435123000",
		"gender": "Male",
		"birth_date": "2002-08-13T21:00:00.000Z",
		"money": 979304
	},
        .
        .
        .
]
```

### `GET /profile/employee`
::::warning
Requires **Admin** permission. Otherwise returns **403**.
::::

#### Output
```json
[
	{
		"national_id": "00000000000",
		"name": "Semih",
		"surname": "Utku",
		"email": "semih.utku@ams.com",
		"phone": "0090000000000",
		"gender": "Male",
		"birth_date": "2000-10-09T21:00:00.000Z",
		"password": "123456",
		"permission": "admin",
		"title": "General Menager"
	}
]
```

### Add Money To User

### `POST /profile/money"`

::::warning
Requires **Admin** or **Seller** permission. Otherwise returns **403**.
::::

::::info
Example: `http://localhost:5500/profile/money`
::::

#### Input
```json
{
  	"money" : "5000",
	"user_id" : "1"
}
```
#### Output
`Succes : 200 , User Not Found : 404 `
```json
{
	"new_balance": 699276
}
```

