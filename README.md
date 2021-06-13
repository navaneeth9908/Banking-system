import Foundation

class Customer
{
    var cif: String
    var fullName: String
    var dob: String
    var occupation: String
    var phoneNumber: Int
    var emailId: String
    
    var address: String
    var panNumber :String
   
    
    var account1: SavingsAccount?
    var account2: CurrentAccount?
    var account3: SalaryAccount?
    
    init (cif:String,fullName:String,DOB:String,occupation:String,phoneNumber:Int,emailId:String,address:String,panNumber:String)
    {
        self.cif = cif
        self.fullName = fullName
        self.dob = DOB
        self.occupation = occupation
        self.phoneNumber = phoneNumber
        self.emailId = emailId
        self.address = address
        self.panNumber = panNumber

        
        self.account1 = nil
        self.account2 = nil
        self.account3 = nil
    }

    
    func createAccount(type:Int, accountNumber:String, initialamount:Double) -> Bool{
        
        if getAccount(type: type) == nil {
            switch type
            {
                case 1:
                    if initialamount >= 0{
                        self.account1 = SavingsAccount(accountNumber:accountNumber, initialDeposit: initialamount)
                        print("Savings Account Creation Successful\nCurrent Balance: \(self.account1?.currentBalance ?? 0.0)\n")
                        return true
                    }else{
                        print("Account Creation failed")
                        print("Amount must be not Negative")
                        return false
                    }
                case 2:
                    if initialamount >= 5000{
                        self.account2 = CurrentAccount(accountNumber:accountNumber, initialDeposit: initialamount)
                        print("Current Account Creation Successful\nCurrent Balance: \(self.account2?.currentBalance ?? 0.0)\n")
                        return true
                    }else{
                        print("Account Creation failed")
                        print("Minimum Balance for Current Account is 5000")
                        return false
                    }
                case 3:
                    if initialamount >= 0{
                        print("Enter company name:")
                        let companyName = readLine()!
                        print("Enter Employee Id:")
                        let empId = readLine()!
                        self.account3 = SalaryAccount(accountNumber:accountNumber, initialDeposit: initialamount, companyName: companyName, empId: empId)
                        print("Salary Account Creation Successful\nCurrent Balance: \(self.account3?.currentBalance ?? 0.0)\n")
                        return true
                    }else{
                        print("Account Creation failed")
                        print("Amount must be not Negative")
                        return false
                    }
                default:
                    print("**Please Enter valid Account Type**")
                    return false
            }
        }else{
            print("**Entered Account Type already Exists for the user**")
            print("**Please Select Other Account Type **")
            return false
        }
    }
 func getData()
    {
        print("---------------Customer Information Form(cif no):\(self.cif)-----------------")
        print("1.Customer Name:\(self.fullName)\n2.Date of Birth:\(self.dob)\n3.Occupation:\(self.occupation)\n4.Phone Number:\(self.phoneNumber)\n5.Email Id:\(self.emailId)\n6.Address:\(self.address)\n7.PAN-Number:\(self.panNumber)\n")
        print("Customer have following Accounts with us")
        if self.account1 != nil {
            print("Account No: \(account1!.accountNo)")
            print("Type: \(self.account1!.type)")
        }
        if self.account2 != nil {
            print("Account No: \(account2!.accountNo)")
            print("Type: \(self.account2!.type)")
        }
        if self.account3 != nil {
            print("Account No: \(account3!.accountNo)")
            print("Type: \(self.account3!.type)")
            print("Company Name: \(self.account3!.companyName)")
            print("Employee Id: \(self.account3!.empId)")
        }
    }
    
    func getUserAccounts()->Bool{
        var count = 0
        if self.account1 != nil || self.account2 != nil || self.account3 != nil{
            print("\nCustomer have Following Accounts with us")
            if self.account1 != nil {
                print("1.\(self.account1!.type)")
                count += 1
            }
            if self.account2 != nil {
                print("2.\(self.account2!.type)")
                count += 1
            }
            if self.account3 != nil {
                print("3.\(self.account3!.type)")
                count += 1
            }
            print("\nPlease Select Account type")
            return true
        }else {
            print("Customer have No accounts Created")
            return false
        }
                
    }
    
    func getAccount(type:Int) -> Account?{
        switch type
        {
            case 1:
                if self.account1 != nil{
                    return self.account1
                }else {
                    return nil
                }
            case 2:
                if self.account2 != nil{
                    return self.account2
                }else {
                    return nil
                }
            case 3:
                if self.account3 != nil{
                    return self.account3
                }else {
                    return nil
                }
            default:
                print("**Please Enter a Valid Account type to Proceed**")
                return nil
        }
    }
    
    func checkBalance(type: Int)
    {
        let account = getAccount(type:type)
        if account != nil {
            print("Account Balance is: \(account!.currentBalance)")
        }else{
            print("**Invalid Account type**")
        }
    }
    
    func withdrawMoney(from:Int,amount:Double) -> Bool
    {
        let account = getAccount(type:from)
        if account != nil {
            if account!.withDraw(amount:amount) {
                print("Amount withdrawal successfull")
                return true
            }else {
                return false
            }
        }else{
            print("Please Enter Valid Account Type")
                return false
        }
    }
    
    
    func depositMoney(to:Int,amount:Double)->Bool
    {
        let account = getAccount(type:to)
        if account != nil {
            if account!.deposit(amount:amount) {
                print("Amount deposit successfull")
                return true
            }else {
                return false
            }
        }else{
            print("Please Enter Valid Account Type")
            return false
        }
    }
    
    
    func transfermoney(from:Int,to:Int,amount:Double)->Bool
    {
        var check = true
            let wAccount = getAccount(type:from)
            let dAccount = getAccount(type:to)
            if wAccount != nil
            {
                if dAccount != nil{
                    if amount <= wAccount!.currentBalance{
                        wAccount!.currentBalance -= amount
                        print("Current Balance in \(wAccount!.type):\(wAccount!.currentBalance)\n")
                        check = true
                    }else{
                        print("Insufficient Balance in \(wAccount!.type) \(wAccount!.currentBalance)\n")
                        check = false
                    }
                    if check == true{
                        let dAccount = getAccount(type:to)
                        if dAccount != nil{
                                dAccount!.currentBalance += amount
                                print("Current Balance in \(dAccount!.type):\(dAccount!.currentBalance)\n")
                        }
                    }
                }else{
                    print("Beneficiary Account Doesn't Exists")
                }
            }else{
                print("User Account Doesn't Exists")
            }
            
        return check
    }
    
    func paybills(from:Int) -> Bool
    {
        print("\nChoose anyone from below:\n 1:Electricity Bill\n 2.BroadBand Bill \n 3.Credit Bill\n 4.DTH Bill")
        let choice = Int(readLine()!) ?? 0
        switch choice{
        
            case 1:
                print("\nPlease enter Electricity Bill Id")
                _ = readLine()!
                let randomInt = Int.random(in: 100..<10000)
                let bill = Double(randomInt)
                print("\nYour Electricity Bill is : \(bill)")
                print("\nDo you want to pay\n 1:Yes\n 2:No\nSelect 1 or 2 only")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:bill) {
                            print("Payment Succesfull")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else {
                    return false
                }
                
            case 2:
                print("\nPlease enter Broadband Bill Id")
                _ = readLine()!
                let randomInt = Int.random(in: 400..<3000)
                let bill = Double(randomInt)
                print("\nYour BroadBand Bill is : \(bill) ")
                print("\nDo you want to pay\n 1:Yes\n 2:No\nSelect 1 or 2 only")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:bill) {
                            print("Payment Succesfull")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else {
                    return false
                }
                
            
                
                
            case 3:
                let randomInt = Int.random(in: 0..<1000)
                let bill = Double(randomInt)
                print("\nYour Credit Bill is : \(bill) ")
                print("\nDo you want to pay\n 1:Yes\n 2:No\nSelect 1 or 2 only")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:bill) {
                            print("Payment Succesfull")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else{
                    return false
                }
                
            case 4:
                print("\nPlease enter DTH Bill Id")
                _ = readLine()!
                let randomInt = Int.random(in: 300..<1000)
                let bill = Double(randomInt)
                print("\nYour DTH Bill is : \(bill) ")
                print("\nDo you want to pay\n 1:Yes\n 2:No\nSelect 1 or 2 only")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:300.0) {
                            print("Payment Succesfull")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else{
                    return false
                }
                
            default:
                print("\nInvalid Selection")
                return false
        }
        
    }
    
    func bookings(from:Int) -> Bool
    {
        print("\nChoose anyone from below:\n 1:Movie Tickets \n 2.Travel Booking\n ")
        let choice = Int(readLine()!)!
        switch choice{
        
            case 1:
                print("\nSelect Movie:\n 1. Avatar \n 2.Spider man\n 3.Anabelle\n 4.Justice league")
                _ = Int(readLine()!) ?? 2
                print("Enter No of Tickets")
                let num = Double(readLine()!) ?? 1.0
                print("Each Ticket Fare is : 300 ")
                print("Total Fare:\(300*num)")
                print("\nDo you want to pay\n 1:Yes\n 2:No\nSelect 1 or 2 only")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    print("Booking Ticket...")
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:300.0*num) {
                            print("Booking Succesfull \n Theatre: Imax\t\t\tSeat No: L7")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else{
                    return false
                }
                
            case 2:
                print("\nSelect Mode of Travel: \n1.Flight \n2.Bus \n3.Train")
                _ = Int(readLine()!) ?? 2
                print("Enter Source City:")
                _ = readLine()!
                print("Enter Destination City:")
                _ = readLine()!
                print("Enter Date of Travel")
                _ = readLine()
                let randomInt = Int.random(in: 1000..<10000)
                let fare = Double(randomInt)
                print("Your Ticket Fare is : \(fare)")
                print("\nDo You want to pay\n 1:Yes\n 2:No\nSelect 1 or 2")
                let ans = Int(readLine()!) ?? 0
                if ans == 1{
                    print("Booking Ticket...")
                    let account = getAccount(type:from)
                    if account != nil {
                        if account!.withDraw(amount:fare) {
                            print("Booking Succesfull \n Booking ID: \(Int.random(in: 11111..<99999))")
                            return true
                        }else {
                            return false
                        }
                    }else{
                        return false
                    }
                }else{
                    return false
                }
                
           
            default:
                print("**Invalid Selection**")
                return false
        }
        
      }
    
    
    }
    
    
    import Foundation

class Account
{
    var accountNo: String
    var currentBalance: Double = 0.0
    var minBalance: Double = 0.0
    var maxDeposit: Double = 0.0
    var maxWithdrawal: Double = 0.0
    var type: String = ""
    
    init (accountNumber:String,initialDeposit:Double){
        self.accountNo = accountNumber
        self.currentBalance = initialDeposit
    }
    
    func deposit(amount:Double) -> Bool{
        if amount <= self.maxDeposit {
            self.currentBalance += amount
            print("Current Balance:\(self.currentBalance)\n")
            return true
        }
        else{
            print("Transaction UnSuccessfull")
            print("Your Account type can only deposit maximum of \(self.maxDeposit)\n")
            return false
        }
    }

     func withDraw(amount:Double) ->Bool{
        if amount <= currentBalance{
            if amount <= self.maxWithdrawal{
                self.currentBalance -= amount
                print("Current Balance:\(self.currentBalance)")
                return true
            }else{
                print("Transaction UnSuccessfull")
                print("Your Account type can only withdraw maximum of \(self.maxWithdrawal)\n")
                return false
            }
        }else{
            print("Transaction UnSuccessfull")
            print("Balance Insufficient, Current Balance: \(self.currentBalance)\n")
            return false
        }
    }
    
    
}





class SavingsAccount: Account
{

    override init (accountNumber:String,initialDeposit:Double)
    {
        super.init(accountNumber:accountNumber,initialDeposit:initialDeposit)
        self.minBalance = 0.0
        self.maxDeposit = 1000000.0
        self.maxWithdrawal = 1000000.0
        self.type = "Savings Account"
    }
}







class CurrentAccount: Account
{
    override init (accountNumber:String,initialDeposit:Double)
    {
        super.init(accountNumber:accountNumber,initialDeposit:initialDeposit)
        self.minBalance = 3000.0
        self.maxWithdrawal = 3500000.0
        self.maxDeposit = 3500000.0
        self.type = "Current Account"
        
    }
}





class SalaryAccount: Account
{
    var empId: String
    var companyName: String
    
    init (accountNumber:String, initialDeposit:Double, companyName:String, empId:String)
    {
        self.companyName = companyName
        self.empId = empId

        super.init(accountNumber:accountNumber,initialDeposit:initialDeposit)
        self.minBalance = 0.0
        self.maxDeposit = 10000000.0
        self.maxWithdrawal = 10000000.0
        self.type = "Salary Account"
        
    }
}
    
    import Foundation
//*************   main function***************************//
var customers = [Customer]()
var users = 1000
var accountNos = 100000

func getCustomer(cif:String) -> Customer? {
    for i in customers{
        if i.cif == cif {
            return i
        }
    }
    return nil
}

func mainMenu(cus:Customer){
    
    print("\nPlease Select from below to continue\n 1:Create another Account type for same User\n 2:Banking\n 3:Edit your Details\n 4:Logout\nSelect 1 or 2 or 3 or 4 only")
    let choice = Int(readLine()!) ?? 0
    switch choice{
        case 1:
            var rep:Int
            if cus.getAccount(type: 1) != nil && cus.getAccount(type: 2) != nil && cus.getAccount(type: 3) != nil{
                print("Customer already have all Account Types")
            }else{
                repeat{
                    rep = 0
                    print("""
                        \nSelect Account Type\n
                        1.Savings:
                            Featues: -> Basic Account with minimum Balalce of 0 Rupees.
                                     -> Maximum withdrawl and deposit limit of 1000000 Rupees.\n
                        2.Current Account:
                            Featues: -> Current Account with minimum Balalce of 3000 Rupees.
                                     -> Maximum withdrawl and deposit limit of 3500000 Rupees.\n
                        3.Salary Account:
                            Features: -> For Salaried people no minimum balance
                                      -> Maximum withdrawl and deposit limit of 10000000 Rupees.\n
                        Reply with 1 or 2 or 3 only.\n
                        """)
                    let account = Int(readLine()!) ?? 0
                    print("Enter Initial Deposit Amount")
                    let amount = Double(readLine()!) ?? 0.0
                    accountNos += 1
                    let accountNo = String(accountNos)
                    if cus.createAccount(type: account, accountNumber: accountNo, initialamount: amount) == false {
                        rep = 1
                    }
                }while rep == 1
                cus.getData()
            }
            mainMenu(cus: cus)
        case 2:
            banking(cus: cus)
        case 3:
            editdetails(cus: cus)
        case 4:
            print("\nLogging Out....\nThank you for Banking with Us\n")
        default:
            print("\nPlease Enter Correct Choice")
            mainMenu(cus: cus)
    }
    
}


func adddetails()
{
    users += 1
    let cif = String(users)
    
    print("Enter the Full Name")
    let fullname = readLine()!
    
    print("Enter the Date of Birth(DD-MM-YYYY)")
    let Dob = readLine()!
    
    print("Enter the occupation")
    let occupation = readLine()!
    
    print("Enter the phone number")
    let phNo = Int(readLine()!) ?? 1234567891
    
    print("Enter the Email ID")
    let email = readLine()!
    
    print("Enter the address")
    let address = readLine()!
    
    print("Enter the Pan Number")
    let panNo = readLine()!
    
    
    customers.append(Customer(cif:cif,fullName:fullname,DOB:Dob,occupation:occupation,phoneNumber:phNo,emailId:email,address:address,panNumber:panNo))
    
    
    let cus:Customer? = getCustomer(cif: cif)
    if cus != nil{
        var rep:Int
        if cus!.getAccount(type: 1) != nil && cus!.getAccount(type: 2) != nil && cus!.getAccount(type: 3) != nil{
            print("Customer already created all Account Types")
        }else{
            repeat{
                rep = 0
                print("""
                    \nSelect Account Type\n
                    1.Savings:
                        Featues: -> Basic Account with minimum Balalce of 0 Rupees.
                                 -> Maximum withdrawl and deposit limit of 1000000 Rupees.\n
                    2.Current Account:
                        Featues: -> Current Account with minimum Balalce of 3000 Rupees.
                                 -> Maximum withdrawl and deposit limit of 3500000 Rupees.\n
                    3.Salary Account:
                        Features: -> For Salaried people no minimum balance
                                  -> Maximum withdrawl and deposit limit of 10000000 Rupees.\n
                    Reply with 1 or 2 or 3 only.\n
                    """)
                let account = Int(readLine()!) ?? 0
                print("Enter Initial Deposit Amount")
                let amount = Double(readLine()!) ?? 0.0
                accountNos += 1
                let accountNo = String(accountNos)
                if cus!.createAccount(type: account, accountNumber: accountNo, initialamount: amount) == false {
                    rep = 1
                }
             }while rep == 1
            cus!.getData()
        }
        mainMenu(cus: cus!)
    }
    
}


func banking(cus:Customer){
    var more = 1
    print("--------------------***************--------------------------")
    print("\nPlease Select from below Transactions:\n 1:Check Balance\n 2.Deposit Money \n 3.Withdraw Money \n 4.Transfer Money\n 5.Pay Bills\n 6.Bookings\n 7.Exit\n")
    let transaction = Int(readLine()!) ?? 0
    switch transaction{
        case 1:
            if cus.getUserAccounts(){
                print("Please Select Account type to Proceed")
                let account = Int(readLine()!) ?? 0
                cus.checkBalance(type:account)
            }
            
        case 2:
            if cus.getUserAccounts(){
                print("Please Select Account type to Proceed")
                let account = Int(readLine()!) ?? 0
                print("Enter Amount")
                let amount = Double(readLine()!) ?? 0.0
                if cus.depositMoney(to:account,amount:amount) == false{
                    print("Money Deposit failed")
                }
            }
        case 3:
            if cus.getUserAccounts(){
                print("Please Select Account type to Proceed")
                let account = Int(readLine()!) ?? 0
                print("Enter Amount")
                let amount = Double(readLine()!) ?? 0.0
                if cus.withdrawMoney(from:account,amount:amount) == false{
                    print("Money Withdrawl failed")
                }
            }
        case 4:
            if cus.getUserAccounts(){
                print("Select From Account type")
                let fromAccount = Int(readLine()!) ?? 0
                print("Select To Account type")
                let toAccount = Int(readLine()!) ?? 0
                if fromAccount != toAccount{
                    print("Enter Amount")
                    let amount = Double(readLine()!) ?? 0.0
                    if cus.transfermoney(from:fromAccount,to:toAccount,amount:amount) == false{
                        print("Money Transfer failed")
                    }
                }else {
                    print("**The user and Beneficiary Accounts are same**")
                }
                
            }
        case 5:
            if cus.getUserAccounts(){
                var rep:Int
                print("Please Select Account type to Proceed")
                let account = Int(readLine()!) ?? 0
                repeat{
                    rep = 0
                    if cus.paybills(from:account) == false{
                        rep = 1
                    }
                    print("\nDo you want to pay more Bills\n 1:Yes\n 2:No\nSelect 1 or 2")
                    rep = Int(readLine()!) ?? 0
                }while rep == 1
            }
        case 6:
            if cus.getUserAccounts(){
                var rep:Int
                print("Please Select Account type to Proceed")
                let account = Int(readLine()!) ?? 0
                repeat{
                    rep = 0
                    if cus.bookings(from:account) == false{
                        rep = 1
                    }
                    print("\nDo you want to do Book more\n 1:Yes\n 2:No\nSelect 1 or 2")
                    rep = Int(readLine()!) ?? 0
                }while rep == 1
            }
        case 7:
            more = 2
            break
        default:
            more = 2
            print("**Please enter valid Choice**")
            banking(cus:cus)
    }
    
    if more != 2 {
        print("\nDo you want to do more transactions\n 1.Yes\n 2.No\nReply with 1 or 2")
        let repe = Int(readLine()!) ?? 0
        if repe == 1 {
            banking(cus: cus)
        }else{
            mainMenu(cus: cus)
        }
    }else{
        mainMenu(cus: cus)
    }

}


func editdetails(cus:Customer){
    cus.getData()
    var rep:Int
    
    repeat {
        rep = 0
        print("Please Select Data to Update details\nReply with Numbers from 1 to 7 only")
        let choice = Int(readLine()!) ?? 0
        
        switch choice {
        case 1:
            print("Please Enter Full Name to Update")
            cus.fullName = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
        case 2:
            print("Please Enter Date of Birth to Update")
            cus.dob = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
        case 3:
            print("Please Enter Occupation to Update")
            cus.occupation = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
        case 4:
            print("Please Enter Phone Number to Update")
            cus.phoneNumber = Int(readLine()!) ?? cus.phoneNumber
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
        case 5:
            print("Please Enter Email Id to Update")
            cus.emailId = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
        case 6:
            print("Please Enter Address to Update")
            cus.address = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            
            
        case 7:
            print("Please Enter PAN Number to Update")
            cus.panNumber = readLine()!
            print("\nDo you want to update more reply\n 1.Yes\n 2.No\nSelect 1 or 2 only")
            rep = Int(readLine()!) ?? 0
            

        default:
            print("**Please enter valid Choice**")
            rep = 1
        }
        
    }while rep == 1
    
    cus.getData()
    
    mainMenu(cus: cus)
    
}




//---------------------------------------User Starts from Here-------------------------------------------------//
var powerOff:Int
repeat{
    powerOff = 1
    print("***************--------   Greetings User Welcome to ICICI Bank  ---------**************")
    print("\nChoose anyone from below\n 1:New User\n 2:Existing User\n 3:Exit \nSelect 1 or 2 or 3 only")
    let choice = Int(readLine()!) ?? 0
    if choice == 1{
        print("\n--------------------------------------------------------------------------")
        print("Please enter your Basic Details to create an Account\n")
        adddetails()
    }else if choice == 2{
        print("\n--------------------------------------------------------------------------")
        print("                    Welcome               ")
        print("Enter Your Customer Information Number(CIF)")
        let cif = String(readLine()!)
        let cus:Customer? = getCustomer(cif: cif)
        if cus != nil{
            print("\nHi \(cus!.fullName)")
            mainMenu(cus: cus!)
        }else{
            print("\nNo Customer Exists with Given CIF")
        }
    }else if choice == 3{
        print("\nGetting Out of Application...\nThank you...! Have a great day...\n")
        powerOff = 0
    }else {
        print("\n**Please enter valid Choice**")
    }
}while powerOff == 1
