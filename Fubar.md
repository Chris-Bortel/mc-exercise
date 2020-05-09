# Fubar
- FUBAR translates to Effed Up Beyond All Recognition
- This is Carol, Bob is watching
- This is the new feature from bob and carol
 here we go again

-THIS IS ALICE HEAR ME ROAR!


'use strict';

var tableEltoTarget = document.getElementById('storeTable');
var storeHoursOpen = ['6am', '7am', '8am', '9am','10am','11am','12pm','1pm','2pm','3pm','4pm','5pm','6pm','7pm'];
var locationList = [];

function Store(city, minCust, maxCust, avgCookieSale) { 
  this.city = city;
  this.minCust = minCust;
  this.maxCust = maxCust;
  this.avgCookieSale = avgCookieSale;
  this.storeHoursOpen = [ '6am','7am','8am','9am','10am','11am','12pm','1pm','2pm','3pm','4pm','5pm','6pm','7pm',];
  this.cookieSalesPerHour = []; 
  this.thisStoresTotalSalesPerDay = 0; 
}

Store.prototype.randCustomers = function () {
  return Math.floor(
    Math.random() * (this.maxCust - this.minCust) + this.minCust
  );
};

Store.prototype.totalSales = function () {
  if (this.cookieSalesPerHour.length === 0) {
    for (var i = 0; i < storeHoursOpen.length; i++) {
      var customersThisHour = this.randCustomers();
      var cookieSalesThisHour = this.avgCookieSale * customersThisHour;
      this.cookieSalesPerHour.push(Math.floor(cookieSalesThisHour));
    }

    for (i = 0; i < this.cookieSalesPerHour.length; i++) {
      this.thisStoresTotalSalesPerDay += this.cookieSalesPerHour[i];
    }
  }
};
//Method to Render

Store.prototype.renderTableRow = function () { 
  var thisCityTrEl = document.createElement('tr');
  var thisCityThEl = document.createElement('th');
  thisCityThEl.textContent = this.city;
  thisCityTrEl.appendChild(thisCityThEl);

  for(var i = 0; i < storeHoursOpen.length ; i++) {
    var thisCityTdEl = document.createElement('td');
    thisCityTdEl.textContent = this.cookieSalesPerHour[i];
    thisCityTrEl.appendChild(thisCityTdEl);
  }

  var thisCitiesTotalTdEl = document.createElement('td');
  thisCitiesTotalTdEl.textContent = this.thisStoresTotalSalesPerDay;
  thisCityTrEl.appendChild(thisCitiesTotalTdEl);
  tableEltoTarget.appendChild(thisCityTrEl);
};

function renderTableHeader(){
  var topHeaderTrEl = document.createElement('tr');
  var topHeaderThEl = document.createElement('th');
  topHeaderTrEl.appendChild(topHeaderThEl); 

  for(var i = 0; i < storeHoursOpen.length ; i++) {
    var topHeaderTdEl = document.createElement('td');
    topHeaderTdEl.textContent = storeHoursOpen[i];
    topHeaderTrEl.appendChild(topHeaderTdEl);
  }
  topHeaderTdEl = document.createElement('td');
  topHeaderTdEl.textContent = 'Total';
  topHeaderTrEl.appendChild(topHeaderTdEl);
  tableEltoTarget.appendChild(topHeaderTrEl);
}

//render footer
function renderTableFooter(){
  var hourlyTotalsTrEl = document.createElement('tr');
  var hourlyTotalsThEl = document.createElement('th');
  hourlyTotalsThEl.textContent = 'Hourly Total';
  hourlyTotalsTrEl.appendChild(hourlyTotalsThEl);

  for(var i = 0; i < storeHoursOpen.length ; i++) {
    var cookieCounter = 0;
    var hourlyTotalsTdEl = document.createElement('td');
    for(var j = 0; j < locationList.length; j++) {
      cookieCounter += locationList[j].cookieSalesPerHour[i]; 
    }
    hourlyTotalsTdEl.textContent = cookieCounter;
    hourlyTotalsTrEl.appendChild(hourlyTotalsTdEl);
  }

  cookieCounter = 0;
  var totalsTotalsTdEl = document.createElement('td');
  for(i = 0; i < locationList.length; i++) {
    cookieCounter += locationList[i].thisStoresTotalSalesPerDay; 
  }

  totalsTotalsTdEl = document.createElement('td');
  totalsTotalsTdEl.textContent = cookieCounter;
  hourlyTotalsTrEl.appendChild(totalsTotalsTdEl);
  tableEltoTarget.appendChild(hourlyTotalsTrEl);
}

locationList.push(new Store('seattle', 23, 65, 6.3));
locationList.push(new Store('tokyo', 3, 24, 1.2));
locationList.push(new Store('dubai', 11, 38, 3.7));
locationList.push(new Store('paris', 3, 24, 1.2));
locationList.push(new Store('lima', 3, 24, 1.2));

function renderStores(){
  renderTableHeader();
  for(var i = 0; i < locationList.length; i++) {
    locationList[i].totalSales();
    locationList[i].renderTableRow();
  }
  renderTableFooter();
}

renderStores();

function handleSubmit(event){
  event.preventDefault();
  var city = event.target.city.value;
  var minCust = parseInt(event.target.minCust.value);
  var maxCust = parseInt(event.target.maxCust.value);
  var avgCookieSale = parseInt(event.target.avgCookieSale.value);

  locationList.push(new Store(city, minCust, maxCust, avgCookieSale));

  event.target.city.value = null;
  event.target.minCust.value = null;
  event.target.maxCust.value = null;
  event.target.avgCookieSale.value = null;

  tableEltoTarget.innerHTML = '';

  renderStores();
}
var userForm = document.getElementById('userForm');
userForm.addEventListener('submit', handleSubmit);
