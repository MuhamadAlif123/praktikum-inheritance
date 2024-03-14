# praktikum-inheritance

**1.** Implementasi kelas `Account`  dan diagram UML:

```java
public class Account {
    protected double balance;

    public Account(double initialBalance) {
        balance = initialBalance;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance += amount;
    }

    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        } else {
            System.out.println("Insufficient funds");
        }
    }
}
```

Penjelasan:
- `balance` adalah atribut yang disimpan dengan sifat protected, sehingga dapat diakses oleh kelas turunan (subclass).
- Konstruktor `Account` menginisialisasi nilai awal `balance`.
- Metode `getBalance` mengembalikan nilai `balance`.
- Metode `deposit` menambahkan jumlah tertentu ke `balance`.
- Metode `withdraw` mengurangi jumlah tertentu dari `balance` jika saldo mencukupi. Jika tidak, cetak pesan "Insufficient funds".


**2.** Implementasi kelas `SavingAccount` yang merupakan turunan dari kelas `Account`:

```java
public class SavingAccount extends Account {
    private double interestRate;

    public SavingAccount(double balance, double interest_rate) {
        super(balance);
        interestRate = interest_rate;
    }

    public double getInterestRate() {
        return interestRate;
    }

    public void setInterestRate(double interest_rate) {
        interestRate = interest_rate;
    }

    public void addInterest() {
        double interest = getBalance() * interestRate / 100;
        deposit(interest);
    }
}
```

Penjelasan:
- Kelas `SavingAccount` adalah turunan dari kelas `Account` menggunakan keyword `extends`.
- Atribut `interestRate` adalah atribut privat yang menyimpan tingkat bunga.
- Konstruktor `SavingAccount` menerima parameter `balance` dan `interest_rate`, kemudian meneruskan `balance` ke konstruktor kelas induk (`Account`) menggunakan `super(balance)` dan menginisialisasi `interestRate` dengan nilai `interest_rate`.
- Metode `getInterestRate` digunakan untuk mendapatkan nilai `interestRate`.
- Metode `setInterestRate` digunakan untuk mengatur nilai `interestRate`.
- Metode `addInterest` menghitung bunga yang akan ditambahkan ke saldo menggunakan rumus `balance * interestRate / 100` dan kemudian menambahkannya ke saldo menggunakan metode `deposit`.

**3.** Implementasi kelas `CheckingAccount` yang merupakan turunan dari kelas `Account`:

```java
public class CheckingAccount extends Account {
    private double overdraftProtection;

    public CheckingAccount(double balance) {
        this(balance, -1.0); // Default overdraftProtection is -1.0, meaning no overdraft protection
    }

    public CheckingAccount(double balance, double protect) {
        super(balance);
        overdraftProtection = protect;
    }

    @Override
    public boolean withdraw(double amount) {
        if (getBalance() >= amount) { // Sufficient funds available
            super.withdraw(amount);
            return true;
        } else {
            double overdraftNeeded = amount - getBalance();
            if (overdraftProtection == -1.0 || overdraftProtection < overdraftNeeded) {
                // Insufficient funds and no overdraft protection or insufficient overdraft protection
                System.out.println("Insufficient funds and overdraft protection");
                return false;
            } else {
                // Sufficient overdraft protection available
                balance = 0.0;
                overdraftProtection -= overdraftNeeded;
                return true;
            }
        }
    }
}
```

Penjelasan:
- Kelas `CheckingAccount` adalah turunan dari kelas `Account` menggunakan keyword `extends`.
- Atribut `overdraftProtection` adalah atribut privat yang menyimpan nilai overdraft protection.
- Terdapat dua constructor: satu dengan satu parameter untuk inisialisasi saldo saja, dan satu dengan dua parameter untuk inisialisasi saldo dan overdraft protection.
- Method `withdraw` di-override untuk melakukan pengecekan saldo dan overdraft protection sebelum menarik dana.
- Jika saldo mencukupi, penarikan dilakukan dan method mengembalikan `true`.
- Jika tidak, method melakukan pengecekan terhadap overdraft protection. Jika tidak ada proteksi atau proteksi tidak mencukupi, penarikan gagal dan method mengembalikan `false`. Jika proteksi mencukupi, saldo diatur menjadi 0 dan proteksi dikurangi sesuai kebutuhan untuk menutupi cerukan, dan method mengembalikan `true`.
  
Dengan menggunakan kelas `CheckingAccount`, Dengan ini dapat melakukan operasi penarikan yang mencakup pengecekan saldo dan proteksi cerukan.

    public class Main {
        public static void main(String[] args) {
            // Test Account
            System.out.println("Testing Account:");
            Account acc = new Account(1000);
            System.out.println("Initial Balance: " + acc.getBalance());
            acc.deposit(500);
            System.out.println("After Deposit: " + acc.getBalance());
            acc.withdraw(300);
            System.out.println("After Withdraw: " + acc.getBalance());
    
            // Test SavingAccount
            System.out.println("\nTesting SavingAccount:");
            SavingAccount savingAcc = new SavingAccount(2000, 5);
            System.out.println("Initial Balance: " + savingAcc.getBalance());
            savingAcc.addInterest();
            System.out.println("After Adding Interest: " + savingAcc.getBalance());
    
            // Test CheckingAccount
            System.out.println("\nTesting CheckingAccount:");
            CheckingAccount checkingAcc = new CheckingAccount(1500, 1000);
            System.out.println("Initial Balance: " + checkingAcc.getBalance());
            checkingAcc.withdraw(2000); // This should fail due to insufficient funds and overdraft protection
            System.out.println("After Withdraw: " + checkingAcc.getBalance());
        }
    }

Kemudian, jalankan kelas Main untuk melihat hasil pengujian dari ketiga jenis akun tersebut.

