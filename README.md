# Golang Assignment - Pemorgraman Berbasis Kerangka Kerja

## Nama : Helsa Sriprameswari Putri
## NRP : 5025221154
## Kelas : D

## What I Learn - Week 10
## Encapsulate Logic with Functions

Example :

```
func bookTicket(remainingTickets uint, userTickets uint, bookings[] string, firstName string, lastName string, email string) {
	remainingTickets = remainingTickets - userTickets

	bookings = append(bookings, firstName + "" + lastName )
	
	fmt.Printf("Thank you %v %v for booking %v tickets. You will receive a confirmation email at %v\n", firstName, lastName, userTickets, email)
	fmt.Printf("%v tickets remaining for %v\n", remainingTickets, conferenceName)
}
```

Fungsi dalam Go dapat juga mereturn lebih dari 1 value. Fungsi dalam Go dipanggil pada fungsi main utama. Agar code lebih clean, digunakan "package level variables" yang didefiniskan di top outside functions sehingga dapat diakses semua functions. Dalam implementasi encapsulate ini terdiri dari beberapa fungsi, yaitu greetUsers, getUserInput, validateUserInput, bookTicket, dan getFirstNames.


## Organize Code with Go Packages

Dalam program Go, bisa mengorganisasi beberapa file go ke dalam beberapa package. Contohnya membuat folder atau package "helper" yang berisi "helper.go"

```
package helper
import ("strings")
```
```
func ValidateUserInput(firstName string, lastName string, email string, userTickets uint, remainingTickets uint) (bool, bool, bool) {
	isValidName := len(firstName) >= 2 && len(lastName) >= 2
	isValidEmail := strings.Contains(email, "@")
	isValidTicketNumber := userTickets > 0 && userTickets <= remainingTickets
	return isValidName, isValidEmail, isValidTicketNumber
} 
```
Fungsi ValidateUserInput akan accessible ke package lain jika kapital. Dalam fungsi main, jika mau mengimpor package dan menggunakan fungsi dari helper.go dilakukan dengan
```
import ("booking-app/helper")
helper.ValidateUserInput(firstName, lastName, email, userTickets,  remainingTickets)
```
## Scope Rules in Go

3 Levels of Scope :

1. Local : digunakan di dalam fungsi saja / block loop

2. Package : digunakan di package yang sama

3. Global : digunakan di semua package

## Maps

Maps unique keys untuk values yang ada dengan data type yang sama
```
var userData = make(map[string]string)
userData["firstName"] = firstName
userData["lastName"] = lastName
userData["email"] = email
userData["numberOfTickets"] = strconv.FormatUint(uint64(userTickets),10)
```

Untuk mengconvert uint menjadi string menggunakan strconv.


## Structs

Menyimpan beberapa data dengan tipe berbeda.
```
type UserData struct {
	firstName string
	lastName string
	email string
	numberOfTickets uint
}
```

## Goroutines - Concurrency in Go

Proses beberapa task bersamaan. Task dapat juga run di background saat masih menjalankan task lainnya (multithreading).

Menggunakan functionality "time" untuk  stop/block eksekusi current thread/goroutine.

Single Thread execution : 
Execute sendTickets, blocks process lain 10 seconds
Code selanjutnya menunggu eksekusi sendTickets 

Separate Thread for sendTickets

```
go sendTicket(userTickets, firstName, lastName, email)
time.Sleep(10 * time.Second)

```

sendTickets task run di background

Waitgroup

Menunggu proses goroutine selesai

```
package sync
wg.Add(1)
wg.Wait()
wg.Done

```

Add : Set jumlah goroutine yang harus ditunggu (counter) di sebelum melakukan proses goroutine <br>
Wait : Blocks goroutine sampai counter = 0 di akhir fungsi main <br>
Done : Decrement counter by 1 (proses goroutine selesai) <br>

## Program Results

### Greet Users
![image](https://github.com/user-attachments/assets/2a4e351a-149b-4c7b-84a5-ba402ab62054)

Mencetak welcome message di fungsi greetUsers(). Kalimat - kalimat tersebut dicetak dengan fmt.Printf dan fmt.Println.

### Getting User Input
![image](https://github.com/user-attachments/assets/57ed27d6-6986-471e-ab8a-ddaef128f861)

Menggunakan function getUserInput() (string, string, string, uint) untuk mengambil input berupa first name, last name, email, 
```
fmt.Println("Enter your first name: ")
fmt.Scan(&firstName)
```

### Validate User Input

![image](https://github.com/user-attachments/assets/aebec767-e0e3-41b8-b0c9-565b4a67aa52)

Menggunakan function validateUserInput() untuk memvalidasi inputan user dengan first dan last name minimal 2 karakter, email contians "@", ticket number tidak melebihi ticket available.
```
isValidName := len(firstName) >= 2 && len(lastName) >= 2
isValidEmail := strings.Contains(email, "@")
isValidTicketNumber := userTickets > 0 && userTickets <= remainingTickets
```
### Booking Process List with Struct
![image](https://github.com/user-attachments/assets/0a577095-ed29-46d4-a18f-5469f9389bc1)


Untuk store data booking, digunakan struct untuk menyimpan data firstname, lastname, email, number of tickets. Kemudia data disimpan di variable bookings. Setelah user melakukan booking, akan ada thank you message dan juga remaining tickets dihitung dengan substract user tickets dari total tickets.
```
var userData = UserData {
		firstName: firstName,
		lastName : lastName,
		email : email,
		numberOfTickets : userTickets,
	}
bookings = append(bookings, userData)
```

### Store First Names with Slice

![image](https://github.com/user-attachments/assets/29cb0890-b9db-4b3b-a3ff-dafbcf684963)
Data kumpulan first name customer disimpan pada suatu slice dan nantinya akan diprint. Slice bersifat dinamis ukurannya bisa tambah/kurang.
```
firstNames := []string{}
	for _, booking := range bookings {
		firstNames = append(firstNames, booking.firstName)
	}
```

### Send Tickets with Go Routine

![image](https://github.com/user-attachments/assets/35311adc-fa02-4550-a202-5078b7782af3)

Untuk melakukan multithreading (run task send tickets yang selain di main), digunakan go routine. Untuk mengimplementasi go routine, ditambahkan wg.Add(1), wg.Wait(), wg.Done.
Dimana :

wg.Add(1) : Set jumlah goroutine yang harus ditunggu (counter) di sebelum melakukan proses goroutine <br>
wg.Wait(): Blocks goroutine sampai counter = 0 di akhir fungsi main <br>
wg.Done : Decrement counter by 1 (proses goroutine selesai) <br>

```
	var wg = sync.WaitGroup{}

	time.Sleep(50 * time.Second)
	var ticket = fmt.Sprintf("%v tickets for %v %v", userTickets, firstName, lastName)
	fmt.Println("##############")
	fmt.Printf("Sending tickets:\n %v \nto email address %v\n", ticket,email)
	fmt.Println("##############")
	wg.Done()
```

