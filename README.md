Download Link: https://assignmentchef.com/product/solved-oop244-workshop8-virtual-functions
<br>
In this workshop, you are to implement an abstract definition of behavior for a specific type.

<strong>LEARNING OUTCOMES </strong>

Upon successful completion of this workshop, you will have demonstrated the abilities to

<ul>

 <li>define a pure virtual function</li>

 <li>code an abstract base class</li>

 <li>implement behavior declared in a pure virtual function</li>

 <li>explain the difference between an abstract base class and a concrete class</li>

 <li>describe what you have learned in completing this workshop<strong> </strong></li>

</ul>

<strong>INTRODUCTION – BANK ACCOUNTS:</strong>

In this workshop, you create an inheritance hierarchy for the bank accounts of a bank’s clients. All clients can deposit (i.e., credit) money into their accounts and withdraw (i.e., debit) money from their accounts. Two specific types of accounts exist. <em>Savings</em> <em>accounts</em> earn interest on the money they hold. <em>Checking</em> <em>accounts</em> charge a fee per transaction (i.e., for every credit and debit).

<strong>Class Hierarchy:</strong>

The design of your <strong>Account</strong> hierarchy is illustrated in the Figure on the right. An interface named <strong>iAccount</strong> exposes the hierarchy’s functionality to a client module that uses its features. The abstract base class named <strong>Account</strong> holds the balance for an account, can credit and debit an account transaction and can expose the current balance in the account. The two account types derive from this base class. The <strong>SavingsAccount</strong> class and the <strong>ChequingAccount</strong> class inherit the properties and functionality of the <strong>Account</strong> base class.

<strong>IN-LAB </strong>

For the in-lab part of this workshop, you are to code three classes:

<ol>

 <li><strong>iAccount</strong> – the interface to your hierarchy – store it in a file named <strong>h</strong></li>

 <li><strong>Account</strong> – an abstract base class that manages the common operations – store its definition and implementation in files named <strong>h</strong> and <strong>Account.cpp</strong>. In a separate file named <strong>Allocator.cpp</strong> code the function that allocates dynamic memory for an account based on its dynamic type.</li>

 <li><strong>SavingsAccount</strong> – a concrete class – store its definition and implementation in files named <strong>h</strong> and <strong>SavingsAccount.cpp</strong>.</li>

</ol>

<strong>i</strong><strong>Account Interface:</strong><strong>                         </strong>

The <strong>iAccount</strong> interface includes the following pure virtual public member functions:

<ul>

 <li>bool credit(double) – adds a positive amount to the account balance</li>

 <li>bool debit(double) – subtracts a positive amount from the account balance</li>

 <li>void monthEnd() – applies month-end transactions to the account</li>

 <li>void display(std::ostream&amp;) const – inserts account information into an ostream object</li>

</ul>

This interface also declares a public empty virtual destructor.

This interface also declares the following helper function:

<ul>

 <li><strong>iAccount</strong>* CreateAccount(const char*, double) – receives a C-style string identifying the type of account and the initial account balance, creates the account with the starting balance and returns its address.</li>

</ul>

<strong>Account Class:</strong><strong>                               </strong>

The <strong>Account</strong> class derives from the <strong>iAccount</strong> interface, holds the current balance and includes the following public member functions:

<ul>

 <li>Account(double) – constructor receives either a double holding the initial account balance or nothing. If the amount received is not positive-valued or no amount is received, this function initializes the current balance to 0.0. If the amount received is positive-valued, this function initializes the current account balance to the received amount.</li>

 <li>bool credit(double) – receives an amount to be credited (added) to the account balance and returns the success of the transaction. If the amount received is positive-valued, the transaction succeeds and this function adds the amount received to the current balance. If the amount is not positive-valued, the transaction fails and this function does not add the amount received.</li>

 <li>bool debit(double) – receives an amount to be debited (subtracted) from the account balance and returns the success of the transaction. If the amount received is positive-valued, the transaction succeeds and this function subtracts the amount received from the current balance. If the amount is not positive-valued, the transaction fails and this function does not subtract the amount received.</li>

</ul>

The <strong>Account</strong> class includes the following protected member function:

<ul>

 <li>double balance() const – returns the current balance of the account.</li>

</ul>

<strong>Account Class – Allocator Module:</strong><strong>        </strong>

The <strong>Allocator</strong> module defines the account rates and charges and a global function that creates the <strong>Account</strong> object for any type of account currently available in the hierarchy. The rates and charges are

<ul>

 <li>interest rate 0.05 (5%)</li>

 <li>other rates and charges added with other accounts The allocator function has the following specification:</li>

 <li>iAccount* CreateAccount (const char*, double) – this function receives the address of a C-style string that identifies the type of account to be created and the initial balance in the account and returns its address to the calling function. If the initial character of the string is ‘S’, this function creates a savings account in dynamic memory. If the string does not identify a type that is available, this function returns nullptr.</li>

</ul>

You will upgrade this module as you add new account types to the hierarchy with corresponding rates and charges.

<strong>SavingsAccount Class:</strong><strong>                 </strong>

The <strong>SavingsAccount</strong> class derives from the <strong>Account</strong> class and holds the interest rate that applies to the account. This class includes the following public member functions:

<ul>

 <li>SavingsAccount(double, double) – constructor receives a double holding the initial account balance and a double holding the interest rate to be applied to the balance. If the interest rate received is positive-valued, this function stores the rate. If not, this function stores 0.0 as the rate to be applied.</li>

 <li>void monthEnd() – this modifier calculates the interest earned on the current balance and credits the account with that interest.</li>

 <li>void display(std::ostream&amp;) const – receives a reference to an ostream object and inserts the following output on separate lines to the object. The values marked in red are fixed format with 2 decimal places and no fixed field width:</li>

</ul>

o Account type: Savings o Balance: $xxxx.xx o Interest Rate (%): x.xx

The following source code uses your hierarchy:

<table width="673">

 <tbody>

  <tr>

   <td colspan="2" width="673">#include &lt;iostream&gt;#include &lt;cstring&gt;#include “iAccount.h”using namespace sict; using namespace std; // display inserts account information for client//void display(const char* client, iAccount* const acct[], int n) {int lineLength = strlen(client) + 22;cout.fill(‘*’);         cout.width(lineLength);         cout &lt;&lt; “*” &lt;&lt; endl;cout &lt;&lt; “DISPLAY Accounts for ” &lt;&lt; client &lt;&lt; “:” &lt;&lt; endl;         cout.width(lineLength);    cout &lt;&lt; “*” &lt;&lt; endl;         cout.fill(‘ ‘);for (int i = 0; i &lt; n; ++i) {acct[i]-&gt;display(cout);if (i &lt; n – 1) cout &lt;&lt; “———————–” &lt;&lt; endl;}cout.fill(‘*’);         cout.width(lineLength);cout &lt;&lt; “****************************” &lt;&lt; endl &lt;&lt; endl;         cout.fill(‘ ‘);} // close a client’s accounts//void close(iAccount* acct[], int n) {for (int i = 0; i &lt; n; ++i) {             delete acct[i];acct[i] = nullptr;}}int main() {// Create Accounts for Angelina         iAccount* Angelina[2]; // initialize Angelina’s Accounts</td>

  </tr>

  <tr>

   <td width="50">                   }</td>

   <td width="623">Angelina[0] = CreateAccount(“Savings”, 400.0); Angelina[1] = CreateAccount(“Savings”, 400.0); display(“Angelina”, Angelina, 2);cout &lt;&lt; “DEPOSIT $2000 into Angelina’s Accounts …” &lt;&lt; endl; for (int i = 0; i &lt; 2; i++)Angelina[i]-&gt;credit(2000);cout &lt;&lt; “WITHDRAW $1000 and $500 from Angelina’s Accounts … ” &lt;&lt; endl;Angelina[0]-&gt;debit(1000); Angelina[1]-&gt;debit(500); cout &lt;&lt; endl; display(“Angelina”, Angelina, 2);Angelina[0]-&gt;monthEnd(); Angelina[1]-&gt;monthEnd(); display(“Angelina”, Angelina, 2); close(Angelina, 2);</td>

  </tr>

  <tr>

   <td width="50"> </td>

   <td width="623"> </td>

  </tr>

 </tbody>

</table>

The following is the exact output of this tester program:

<table width="673">

 <tbody>

  <tr>

   <td width="673">****************************** DISPLAY Accounts for Angelina:******************************Account type: SavingsBalance: $500.00Interest Rate (%): 5.00———————–Account type: SavingsBalance: $500.00Interest Rate (%): 5.00****************************** DEPOSIT $2000 into Angelina’s Accounts …WITHDRAW $1000 and $500 from Angelina’s Accounts … ****************************** DISPLAY Accounts for Angelina:******************************Account type: SavingsBalance: $1500.00Interest Rate (%): 5.00———————–Account type: SavingsBalance: $2000.00Interest Rate (%): 5.00****************************** ****************************** DISPLAY Accounts for Angelina:******************************Account type: SavingsBalance: $1575.00Interest Rate (%): 5.00———————–</td>

  </tr>

 </tbody>

</table>

Account type: Savings

Balance: $2100.00

Interest Rate (%): 5.00 ******************************

<strong>IN-LAB SUBMISSION  </strong>

Upload <strong>iAccount.h</strong>, <strong>Account.h</strong>, <strong>Account.cpp.</strong>, <strong>Allocator.cpp</strong>,<strong> SavingsAccount.h,</strong> and <strong>SavingsAccount.cpp </strong>to your matrix account. Compile and run your code and make sure everything works properly.

Then, run the following command from your account (use your professor’s Seneca userid to replace profname.proflastname, and your section ID to replace XXX, i.e., SAA, SBB, etc.):

<h2>~profname.proflastname/submit 244XXX_w8_lab&lt;<sub>ENTER&gt;</sub></h2>

and follow the instructions generated by the command and your program.

<strong>I</strong><strong>MPORTANT</strong>: Please note that a successful submission does not guarantee full credit for this workshop. If the professor is not satisfied with your implementation, your professor may ask you to resubmit. Resubmissions will attract a penalty.

<strong>           </strong>

<strong>AT-HOME (30%):</strong>

This part of Wworkshop 8 adds a <strong>ChequingAccount</strong> to your <strong>Account</strong> hierarchy.

Copy the <strong>iAccount.h</strong>, <strong>Account.h</strong>, <strong>Account.cpp.</strong>, <strong>Allocator.cpp</strong>,<strong> SavingsAccount.h, </strong>and <strong>SavingsAccount.cpp</strong> files from the in lab part of your solution.

<strong>ChequingAccount Class:</strong><strong>              </strong>

The <strong>ChequingAccount</strong> class derives from the <strong>Account</strong> class and holds the transaction fee and month-end fee to be applied to the account. This class includes the following public member functions:

<ul>

 <li>ChequingAccount(double, double, double) – constructor receives a double holding the initial account balance, a double holding the transaction fee to be applied to the balance and a double holding the month-end fee to be applied to the account. If the transaction fee received is positive-valued, this function stores the fee. If not, this function stores 0.0 as the fee to be applied. If the monthly fee received is positive-valued, this function stores the fee. If not, this function stores 0.0 as the fee to be applied.</li>

 <li>bool credit(double) – this modifier credits the balance by the amount received and if successful debits the transaction fee from the balance. This function returns true if the transaction succeeded; false otherwise.</li>

 <li>bool debit(double) – this modifier debits the balance by the amount received and if successful debits the transaction fee from the balance. This function returns true if the transaction succeeded; false otherwise.</li>

 <li>void monthEnd() – this modifier debits the monthly fee from the balance, but does not charge a transaction fee for this debit.</li>

 <li>void display(std::ostream&amp;) const – receives a reference to an ostream object and inserts the following output on separate lines to the object. The values marked in red are fixed format with 2 decimal places and no fixed field width:</li>

</ul>

o Account type: Chequing o Balance: $xxxx.xx o Per Transaction Fee: x.xx o Monthly Fee: x.xx




<strong>Account Class – Allocator Module:</strong><strong>        </strong>

The <strong>Allocator</strong> module pre-defines the accounts rates and charges and defines the global function that creates the <strong>Account</strong> object from the types of account currently available. The rates and charges are

<ul>

 <li>interest rate 0.05 (5%)</li>

 <li>transaction fee 0.50</li>

 <li>monthly fee 2.00</li>

</ul>

Modify the allocation function to include the following specification:

<ul>

 <li>iAccount* CreateAccount (const char*, double) – this function receives the address of a C-style string that identifies the type of account to be created and the initial balance in the account and returns its address to the calling function. If the initial character of the string is ‘S’, this function creates a savings account in dynamic memory. <em>If the initial character of the string is ‘C’, this function creates a chequing account in dynamic memory.</em> If the string does not identify a type that is available, this function returns nullptr.</li>

</ul>




The following code uses your hierarchy:

<table width="673">

 <tbody>

  <tr>

   <td width="673">#include &lt;iostream&gt;#include &lt;cstring&gt;#include “iAccount.h”using namespace sict; using namespace std; // display inserts account information for client//void display(const char* client, iAccount* const acct[], int n) {int lineLength = strlen(client) + 22;cout.fill(‘*’);      cout.width(lineLength);    cout &lt;&lt; “*” &lt;&lt; endl;         cout &lt;&lt; “DISPLAY Accounts for ” &lt;&lt; client &lt;&lt; “:” &lt;&lt; endl;      cout.width(lineLength);    cout &lt;&lt; “*” &lt;&lt; endl;         cout.fill(‘ ‘);for (int i = 0; i &lt; n; ++i) {acct[i]-&gt;display(cout);if (i &lt; n – 1) cout &lt;&lt; “———————–” &lt;&lt; endl;}cout.fill(‘*’);         cout.width(lineLength);</td>

  </tr>

  <tr>

   <td width="673">       cout &lt;&lt; “****************************” &lt;&lt; endl &lt;&lt; endl;         cout.fill(‘ ‘);} // close a client’s accounts//  void close(iAccount* acct[], int n) {for (int i = 0; i &lt; n; ++i) {delete acct[i];acct[i] = nullptr;}}int main () {// Create Accounts for Angelina         iAccount* Angelina[2]; // initialize Angelina’s AccountsAngelina[0] = CreateAccount(“Savings”, 400.0);          Angelina[1] = CreateAccount(“Chequing”, 400.0);         display(“Angelina”, Angelina, 2); cout &lt;&lt; “DEPOSIT $2000 into Angelina’s Accounts …” &lt;&lt; endl ;         for(int i = 0 ; i &lt; 2 ; i++)Angelina[i]-&gt;credit(2000); cout &lt;&lt; “WITHDRAW $1000 and $500 from Angelina’s Accounts … ” &lt;&lt; endl ;Angelina[0]-&gt;debit(1000);  Angelina[1]-&gt;debit(500);cout &lt;&lt; endl;display(“Angelina”, Angelina, 2); Angelina[0]-&gt;monthEnd();  Angelina[1]-&gt;monthEnd();         display(“Angelina”, Angelina, 2); close(Angelina, 2);}</td>

  </tr>

 </tbody>

</table>

The following is the exact output of the tester program:

<table width="673">

 <tbody>

  <tr>

   <td width="673">****************************** DISPLAY Accounts for Angelina:******************************Account type: SavingsBalance: $500.00Interest Rate (%): 5.00———————–Account type: Chequing Balance: $500.00Per Transaction Fee: 0.50Monthly Fee: 2.00****************************** DEPOSIT $2000 into Angelina’s Accounts …WITHDRAW $1000 and $500 from Angelina’s Accounts … </td>

  </tr>

 </tbody>

</table>

****************************** DISPLAY Accounts for Angelina:

******************************

Account type: Savings

Balance: $1500.00

Interest Rate (%): 5.00

———————–

Account type: Chequing

Balance: $1999.00

Per Transaction Fee: 0.50

Monthly Fee: 2.00

******************************

****************************** DISPLAY Accounts for Angelina:

******************************

Account type: Savings

Balance: $1575.00

Interest Rate (%): 5.00

———————–

Account type: Chequing

Balance: $1997.00

Per Transaction Fee: 0.50

Monthly Fee: 2.00

******************************

<h2>REFLECTION</h2>

Study your final solution, reread the related parts of the course notes, and make sure that you have understood the concepts covered by this workshop. <strong>This should take no less than 30 minutes of your time.</strong>

Create a file named reflect.txt that contains your <strong>detailed description of the topics that you have learned </strong>in completing this workshop and mention any issues that caused you difficulty. Include in your explanation—<strong>but do not limit it to</strong>—the following points:

<ol>

 <li>What is the difference between an abstract base class and a concrete class?</li>

 <li>Identify the functions that shadow functions of the same name in your solution?</li>

 <li>Explain what have you learned in this workshop.</li>

</ol>

<h3>QUIZ REFLECTION</h3>

Add a section to reflect.txt called Quiz X Reflection. Replace the X with the number of the last quiz that you received and list the numbers of all questions that you answered incorrectly.

Then for each incorrectly answered question write your mistake and the correct answer to that question. If you have missed the last quiz, then write all the questions and their answers.

<h3>AT-HOME SUBMISSION</h3>

Upload <strong>w8_at_home.cpp</strong>, <strong>iAccount.h</strong>, <strong>Account.h</strong>, <strong>Account.cpp, Allocator.cpp, SavingsAccount.h, </strong><strong>SavingsAccount.cpp, </strong><strong>ChequingAccount.h,</strong> <strong>ChequingAccount.cpp, </strong>and <strong>reflect.txt </strong>to your matrix account. Compile and run your code and make sure everything works properly.

To submit, run the following command from your account (use your professor’s Seneca userid to replace profname.proflastname, and your section ID to replace XXX, i.e., SAA, SBB, etc.):

<h4>~profname.proflastname/submit 244XXX_w8_home&lt;<sub>ENTER&gt;</sub></h4>

and follow the instructions generated by the command and your program.

<strong>I</strong><strong>MPORTANT</strong>: Please note that a successful submission does not guarantee full credit for this workshop. If the professor is not satisfied with your implementation, your professor may ask you to resubmit. Resubmissions will attract a penalty.


