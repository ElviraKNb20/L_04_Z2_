# L_04_Z2_
package bank;

import java.util.Scanner;

public class BankApplication {
    public static void main(String[] args) {
        Bank bank = new Bank();

        // Додамо декілька клієнтів та їх рахунків для прикладу.
        Client client1 = new Client("Client1");
        Account account1 = new Account(1, 1000);
        Account account2 = new Account(2, -500);
        client1.addAccount(account1);
        client1.addAccount(account2);

        Client client2 = new Client("Client2");
        Account account3 = new Account(3, 2000);
        client2.addAccount(account3);

        bank.addClient(client1);
        bank.addClient(client2);

        Scanner scanner = new Scanner(System.in);
        int choice;
        do {
            System.out.println("Bank Menu:");
            System.out.println("1. Calculate Total Balance");
            System.out.println("2. Calculate Total Positive Balance");
            System.out.println("3. Calculate Total Negative Balance");
            System.out.println("0. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    double totalBalance = bank.calculateTotalBalance();
                    System.out.println("Total Balance: " + totalBalance);
                    break;
                case 2:
                    double totalPositiveBalance = bank.calculateTotalPositiveBalance();
                    System.out.println("Total Positive Balance: " + totalPositiveBalance);
                    break;
                case 3:
                    double totalNegativeBalance = bank.calculateTotalNegativeBalance();
                    System.out.println("Total Negative Balance: " + totalNegativeBalance);
                    break;
                case 0:
                    System.out.println("Exiting the program.");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 0);
    }
}

package bank;

public class Account {
    private int accountNumber;
    private double balance;
    private boolean isBlocked;

    public Account(int accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
        this.isBlocked = false;
    }

    public int getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public boolean isBlocked() {
        return isBlocked;
    }

    public void blockAccount() {
        isBlocked = true;
    }

    public void unblockAccount() {
        isBlocked = false;
    }

    public void deposit(double amount) {
        if (!isBlocked) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {
        if (!isBlocked && balance >= amount) {
            balance -= amount;
        }
    }
}

package bank;

import java.util.ArrayList;
import java.util.List;

public class Client {
    private String name;
    private List<Account> accounts;

    public Client(String name) {
        this.name = name;
        this.accounts = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public void addAccount(Account account) {
        accounts.add(account);
    }

    public List<Account> getAccounts() {
        return accounts;
    }
}

package bank;

import java.util.ArrayList;
import java.util.List;

public class Bank {
    private List<Client> clients;

    public Bank() {
        this.clients = new ArrayList<>();
    }

    public void addClient(Client client) {
        clients.add(client);
    }

    public List<Client> getClients() {
        return clients;
    }

    public double calculateTotalBalance() {
        double totalBalance = 0;
        for (Client client : clients) {
            for (Account account : client.getAccounts()) {
                totalBalance += account.getBalance();
            }
        }
        return totalBalance;
    }

    public double calculateTotalPositiveBalance() {
        double totalPositiveBalance = 0;
        for (Client client : clients) {
            for (Account account : client.getAccounts()) {
                if (account.getBalance() > 0) {
                    totalPositiveBalance += account.getBalance();
                }
            }
        }
        return totalPositiveBalance;
    }

    public double calculateTotalNegativeBalance() {
        double totalNegativeBalance = 0;
        for (Client client : clients) {
            for (Account account : client.getAccounts()) {
                if (account.getBalance() < 0) {
                    totalNegativeBalance += account.getBalance();
                }
            }
        }
        return totalNegativeBalance;
    }
}
