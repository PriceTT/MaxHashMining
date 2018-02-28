# MaxHashMining Script to send email if hashrate is below a set value
# 1) Copy script into google sheets script editor 
# 2) In the script editor click on the clock icon in the menu
# 3) Set the project triggers to update every 5 min for myFunction and sendEmail function
# 4) Sit back and relax 
# Tips ETH : 0xa3da841731c79097126796488922c22951d8fd53

// Add function to menu
function onOpen() {
  var ui = SpreadsheetApp.getUi();
  ui.createMenu('MaxHash API Menu')
      .addItem('Get Stats','myFunction')
      .addToUi();
}

// Set time when sheet updates
function onEdit(e) {
    SpreadsheetApp.getActiveSpreadsheet().getRange('A1').setValue(new Date().toTimeString());
  }

// Function that makes API call and parses the data
function myFunction() {

  // Call Maxhash API
  var response = UrlFetchApp.fetch("https://ethpool.maxhash.org/api/accounts/0xa3da841731c79097126796488922c22951d8fd53");
   
  // Copy data tó speadsheet
  var stats = response.getContentText();
  var sheet = SpreadsheetApp.getActiveSheet();
  //sheet.getRange(1,1).setValue([stats]);

  var data = JSON.parse(stats);
  
  //Variable to store contents of json string
  var emp = [];
  emp[0] = [data.currentHashrate];
  emp[1] = [data.hashrate];
  emp[2] = [data.payments];
  emp[3] =([data.paymentsTotal]);
  emp[4] =([data.stats.balance]);
  emp[5] =([data.workers.PriceTT.offline]);
  emp[6] =([data.workersOffline]);
  emp[7] =([data.workersOnline]);
  SpreadsheetApp.flush(); 
  
  return emp

 }
  


function sendEmails() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2;  // First row of data to process
  var numRows = 10;   // Number of rows to process
  // Fetch the range of cells A2:B10
  var dataRange = sheet.getRange(startRow, 1, numRows, 2)
  // Fetch values for each row in the Range.
  var data = dataRange.getValues();
  //for (i in data) {
    
    var HashRate = sheet.getRange(2, 2).getValue();
    // set the limit for your hash rate
    if (HashRate <= 1.20E8) {
  
      var emailAddress = "youremail@gmail.com";  
      var message = "Check hash rate. It is currently "+ HashRate/1e6 +" MH/s https://ethpool.maxhash.org/#/account/0xa3da841731c79097126796488922c22951d8fd53";
      var subject = "Check hash rate. It is currently "+ HashRate/1e6 +" MH/s";
      MailApp.sendEmail(emailAddress, subject, message);
     
   }
  
}
