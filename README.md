# Golang Assignment - Pemorgraman Berbasis Kerangka Kerja

## Nama : Helsa Sriprameswari Putri
## NRP : 5025221154
## Kelas : D


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

Proses beberapa task bersamaan. Task dapat juga run di background saat masih menjalankan task lainnya.

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

Add : Set jumlah goroutine yang harus ditunggu (counter) di sebelum melakukan proses goroutine
Wait : Blocks goroutine sampai counter = 0 di akhir fungsi main
Done : Decrement counter by 1 (proses goroutine selesai)

## Results


