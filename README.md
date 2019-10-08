### UiPath Custom REFramework Template ###
**Custom Robotic Enterprise Framework**

A UiPath Studio template upon which you can build, test and run attended and unattended business processes.
This framework was first a customization of standar REF created through UiPath Studio, see the [oficial repo](https://github.com/UiPath/ReFrameWork). While working in new features I decided to implement some ideas from the [Enhanced-REFramework](https://github.com/mihhdu/Enhanced-REFramework),

Features:
* Low code signature: A handful of clearly written, commented reusable functions that anyone can understand and a clearly commented Main.xaml bringing structure to the process design architecture.
* Separation of concerns: We keep framework implementation separate from business logic code, allowing developers and SMEs alike to focus on building processes.
* Reusability: Works for any type of process, Independent of Data sources (QueueItems, local excel files, Database data, API retrieved data)
* Maintain, extend and upgrade: Easy to maintain, thanks to code lightness and SOC. Extend to achieve process behaviour by editing 6 empty workflows that connect to the Main.xaml in a standard way. Upgrade or extend framework independently of business code, by editing only one file, the Main.xaml.
* Exception recovery and retry: If all recovery options are exhausted, closing and restarting the application environment while remembering data is what humans do too. Top-level exception recovery is managed by the framework layer with Retry rules you can easily configure.
* Audit: Keep track of the robot's work, with as much detail and privacy as you choose with the new Workblock concept; Add business information of your choosing to the published log.


Details:

* composed of workflows that record hierarchical and execution data in a consistent way, termed Workblocks.
* using *State Machine* layout.
* offering high level exception handling and application recovery.
* offer a mechanism to abort process execution in certain cases where continuing is destructive
* keeps external settings in *Data\Config.xlsx* file and Orchestrator assets.
* pulls credentials from *Credential Manager* and Orchestrator assets.
* takes screenshots in case of application exceptions.
* provides extra utility workflows.

### Dependencies ###
* UiPath.Core.Activities
* UiPath.Credentials.Activities
* UiPath.Excel.Activities
* UiPath.Mail.Activities
* UiPath.WebAPI.Activities

### For New Project ###
To begin implementing take the following steps:

1. Check out the *Data\Config.xlsx* file and add/customize any required fields and values. You can always come back to the config later.

2. Decide what the process is going to achieve. Think about what kind of data is moving around in the process.

3. Go in Main.xaml and change the following data types to suit your project needs:
TransactionItem - by default a DataRow represents one transaction worth of data
TransactionData - by default a Datatable represents the collection of transaction data to be used in this process run
Ignore errors being generated for the moment.

4. Open *ProcessLayer\GetSetTransactionData.xaml* and change io_TransactionItem and io_TransactionData datatypes to match the ones you set up in the Main.xaml file.

5. Open *ProcessLayer\ProcessTransaction.xaml* and change io_TransactionItem's datatype to match the one you set up in the Main.xaml file.

6. Open Main.xaml and update the interfaces to the functions in the **GET/SET TRANSACTION DATA** and **PROCESS TRANSACTION** states by clicking the import button. The errors from point 1. should now disappear.

7. Begin writing business code in the workflows located in the *ProcessLayer* folder

*ProcessLayer\CleanupAndPrep.xaml* - write business code that gets called the first time you run the process. By default, KillAllProcesses and DeleteFilesInFolder for *DATA\TEMP* are called here to make sure the app environment is clean.

*ProcessLayer\InitAllApplications.xaml* - write business code to open all applications needed during processing.

*ProcessLayer\CloseAllApplications.xaml* - write business code to close all applications you have opened at previous step.

*ProcessLayer\KillAllProcesses.xaml* - write business code to kill all applications (only invoked when applications not responding).

*ProcessLayer\GetProcessData.xaml* - write business code to fetch transaction data and other data that is required during the process execution.

*ProcessLayer\GetSetTransactionData.xaml* - write business code to fetch data for the current transaction and save/write it to the io_TransactionItem variable. Decide when the process will end by setting io_TransactinItem to Nothing. Make use of external iterator in_TransactionNumber and in_RetryNumber if io_TransactionItem is an element of io_TransactionData. (Ex. io_TransactionItem = io_TransactionData(in_TransactionNumber - 1).

*ProcessLayer\ProcessTransaction.xaml* - write business code to use applications that are open and data from io_TransactionItem and complete the transaction. At the end of the transaction, take the applications to the page they were at when you began the transaction.

8. Execute and have fun!

![MainStateMachine](https://github.com/nerlichman/UiPath-Custom-REFramework/blob/master/MainStateMachine.PNG)

