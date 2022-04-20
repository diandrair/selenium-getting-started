# Mulai Automation Testing dari nol

Ketika memulai sebagai Software tester engineer kamu pasti kewalahan dengan semua terminologi yang kamu dapatkan sebagai seorang pemula. Awalnya semua terlihat menarik namun saat kamu mencoba automation testing disinilah semua terlihat menakutkan, bahkan pada satu waktu kamu bertanya-tanya "Can I handle this?"

Permasalahan utama adalah bagaimana caranya untuk memulai tanpa terpapar terlalu banyak detail tentang automation, oleh karena itu saya akan menjelaskan  dalam satu project step-by-step yang mudah dimengerti, environment yang akan saya gunakan dalam artikel ini :

- IDE: IntelliJ IDEA
- Programming language: Java
- Browser and OS: Chrome on Mac
- Automation Tool: WebDriver + chromedriver
- Framework : Spring boot + Selenium + Cucumber

Basic pengetahuan Java dan OOP konsep akan sangat membantu untuk mengerti logic yang ada pada projek kita.

Shall we start?

-----------------------------------------------------------------------------------------------------------------

## The Basics

Selenium WebDriver adalah tool untuk mengeksekusi flow kerja sebuah Web-Browser secara otomatis. Pastinya sangat melelahkan ketika kita harus mengisi form berulang kali atau harus memverifikasi ratusan halaman web. Automation dimulai dengan memuat halaman web yang spesific, melakukan aksi di halaman tersebut, dan kemudian membandingkan apakah hasil yang muat sesuai harapan kita atau tidak, tentu dalam pelaksanaannya memerlukan pengaturan yang bervariasi tergantung platform yang kita gunakan (e.g Windows, Mac, or Linux).

Umumnya syntax automation akan terlihat seperti ini: 

<img width="589" alt="image" src="https://user-images.githubusercontent.com/77116793/162722968-d33db328-6108-48e6-b3c7-fa2f6caffb3c.png">

Our plan of action on a webpage comes down to this scenario most of the time:

Dengan scenario sebagai berikut:

1. Muat halaman web (halaman yang spesifik)
2. Cari element yang akan dituju. Ada banyak cara untuk identifikasi element dalam sebuah webpage, element identifier itu disebut sebagai Locator. Di artikel ini kita akan menggunakan "Xpath" dan "id" sebagai locator
3. Jalankan aksi pada element tersebut (click, sendkeys, scroll, dll)
4. Validasi hasil yang ditampilkan, kita memastikan apakah actual result sama dengan result yang kita harapkan.


-----------------------------------------------------------------------------------------------------------------

## Our project for Test Automation

![image](https://user-images.githubusercontent.com/77116793/164260026-2011b8ee-7b9a-44aa-b921-056e10ce81cb.png)
![image](https://user-images.githubusercontent.com/77116793/164260342-e0ed4945-ef84-4381-b9ed-4d1155a1f17f.png)

Kita akan meng-automate simple web login dengan outline berikut:

1. Menuju homepage
2. Klik Sign in di homepage
3. Verifikasi halaman login
4. Masukan credential
5. Klik Sign in di halaman login
6. Verifikasi halaman dashboard user

-----------------------------------------------------------------------------------------------------------------

## Setting up the project

Semuanya telah jelas sejauh ini? Baiklah, kalau begitu mari kita siapkan project dan environment kita.

1. Langkah 1 – [Instal Java 11 di komputermu](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html) atau cek apakah java sudah terinstal [disini](https://phoenixnap.com/kb/check-java-version-on-mac-windows)
2. Langkah 2 – [Instal IntelliJ IDE Community Edition](https://www.jetbrains.com/idea/download/) ![image](https://user-images.githubusercontent.com/77116793/164264571-b3065d96-8932-492a-bd52-982925287ed3.png)

Setelah semuanya terinstal di sistem komputer kita, mari kita mulai.
1. Pergi ke [Spring Initializr](https://start.spring.io/) untuk generate project baru
2. Pilih Maven dan Java ![image](https://user-images.githubusercontent.com/77116793/164265401-f94058fc-0a86-4e5f-a2db-1f4624920ec6.png)
3. Pilih nama apa saja untuk project (misalnya "Demo Automation"), pastikan pilih "Jar" lalu tekan "Generate" untuk membuat project, Kamu akan melihat file .zip pada file download, jangan lupa untuk mengekstrak file sebelumnya lanjutkan ke langkah berikutnya.
4. Buka aplikasi IntelliJ dan pilih open ![image](https://user-images.githubusercontent.com/77116793/164266088-cef64915-dd6c-4c2b-9c3b-8928c95d409e.png) 
5. Buka file kamu, lalu pilih file demo yang baru saja di unzip, kamu akan melihat dialog seperti ini, pilih "Trust Project" ![image](https://user-images.githubusercontent.com/77116793/164266304-df4a6caa-f556-4f4f-9f6c-daec17ebd00d.png)
6. Sekarang sudah memiliki template ![image](https://user-images.githubusercontent.com/77116793/164266526-85d268b0-7d66-4fd1-b313-711b1fce5a55.png)
7. Dan setelah beberapa saat, kamu mungkin memiliki pertanyaan tentang Maven. Maven adalah build automation tool, yang digunakan terutama untuk project Java. Kita akan menggunakannya untuk menambahkan library tambahan ke proyek ini 

![image](https://user-images.githubusercontent.com/77116793/164266939-38c16eb6-4a52-40b1-ad20-0056edf877f2.png)

Setelah kita buka project dan membuka file pom.xml akan terlihat seperti ini:

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.3</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demoAutomation</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.6.3</version>
			</plugin>
		</plugins>
	</build>

</project>

```

Sekarang kita dapat menggunakan file dibawah ini untuk menambahkan library yang diperlukan untuk menjalankan otomasi kita. Berikut adalah baris yang harus kita tambahkan ke pom.xml kita :

```
		<!-- to be added under properties section -->
		<selenium.version>3.141.59</selenium.version>
		<webdrivermanager.version>3.8.1</webdrivermanager.version>
		<testng.version>7.1.0</testng.version>

		<!-- to be added under dependencies section after spring-boot-starter-->
		<dependency>
			<groupId>org.seleniumhq.selenium</groupId>
			<artifactId>selenium-java</artifactId>
			<version>${selenium.version}</version>
		</dependency>
		<dependency>
			<groupId>io.github.bonigarcia</groupId>
			<artifactId>webdrivermanager</artifactId>
			<version>${webdrivermanager.version}</version>
		</dependency>
		<!-- testng users only -->
		<dependency>
			<groupId>org.testng</groupId>
			<artifactId>testng</artifactId>
			<version>${testng.version}</version>
			<scope>test</scope>
		</dependency>	

		<!-- to be added under plugin section -->

		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-surefire-plugin</artifactId>
			<version>2.19.1</version>
		</plugin>	
```

Untuk lebih spesifik:

```
Selenium Webdriver: org.seleniumhq.selenium
TestNG (unit testing framework): org.testng
Maven SureFire (easy test plan execution): org.apache.maven.plugins
```
