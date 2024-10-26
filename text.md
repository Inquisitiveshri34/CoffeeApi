order.html



<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Order Page</title>
    <style>
      /* All Your css for Order page goes here  */
    </style>
  </head>
  <body>
    <div class="nav">
      <h1>Web Coffee Shop Menu</h1>
      <a href="./index.html">Home</a>
      <a href="./order.html">Ordered</a>
    </div>
    <h1 id="total">
      Your total order value is :- <span id="order-total">0</span>
    </h1>
    <div id="cupon">
      <input type="text" />
      <button>Add Cupon</button>
    </div>
    <div id="order-container">
      <!-- Here Append All the Order Cards here-->
    </div>
  </body>
  <script>
    let arrayReceived = (localStorage.getItem('key')) ? localStorage.getItem('key') : [];
    let arrayOrdered = arrayReceived.split(",").map(el => Number(el));

    const url = `https://dbioz2ek0e.execute-api.ap-south-1.amazonaws.com/mockapi/get-coffee`;

    const ordered = document.querySelector("#order-container")
    const totalValue = document.querySelector("#order-total")
    totalValue.textContent = 0;

    let showData = (data) => {
      
      let totalCost = 0;
      for (let i = 0; i < data.length; i++){
        const div = document.createElement("div")
        const dataEl = data[i];
        const image = document.createElement("img");
        image.setAttribute("src",dataEl.image);
        image.setAttribute("alt","image");
        const title = document.createElement("h3");
        title.textContent = dataEl.title;
        const pricePara = document.createElement("p")
        let price = dataEl.price;
        pricePara.textContent = price;
        const decQuant = document.createElement("button");
        decQuant.textContent = '-';
        decQuant.classList.add("decQuant");
        decQuant.disabled = true;
        const quant = document.createElement("p");
        const quantity = 1;
        quant.textContent = quantity;
        quant.classList.add("quantity");
        const incQuant = document.createElement("button");
        incQuant.textContent = '+';
        incQuant.classList.add("incQuant");
        const cancelBtn = document.createElement("button");
        cancelBtn.textContent = 'Cancel';
        cancelBtn.classList.add("cancelBtn");
        const brk = document.createElement("br");
        div.appendChild(brk);
        div.appendChild(image)
        div.appendChild(title);
        div.appendChild(pricePara);
        div.appendChild(decQuant);
        div.appendChild(quant);
        div.appendChild(incQuant);
        div.appendChild(cancelBtn); 
        ordered.appendChild(div); 
        totalCost += price*quantity;
        totalValue.textContent = totalCost;



        incQuant.addEventListener("click", ()=>{
          if (quant.textContent == 1){
            quant.textContent++;
            let cost = totalValue.textContent;
            cost = (couponBtn.disabled) ? Math.floor(Number(cost) + Math.floor(price*70/100)) : Number(cost) + price;
            totalValue.textContent = Math.floor(cost);
            decQuant.disabled = false;
          } else {
            quant.textContent++;
            let cost = totalValue.textContent;
            cost = (couponBtn.disabled) ? Math.floor(Number(cost) + Math.floor(price*70/100)) : Number(cost) + price;
            totalValue.textContent = Math.floor(cost);
          }
        })
        decQuant.addEventListener("click",()=>{
          if (quant.textContent > 2) {
            quant.textContent--;
            let cost = totalValue.textContent;
            cost = (couponBtn.disabled) ? Math.floor(Number(cost) - Math.floor(price*70/100)) : Number(cost) - price;
            totalValue.textContent = Math.floor(cost);
          }
          else {
            decQuant.disabled = true;
            quant.textContent--;
            let cost = totalValue.textContent;
            cost = (couponBtn.disabled) ? Math.floor(Number(cost) - (price*70/100)) : Number(cost) - price;
            totalValue.textContent = Math.floor(cost);
          }
        })


        cancelBtn.addEventListener("click",()=>{
          idCancel = dataEl.id;
          let arrayReceived = (localStorage.getItem('key')) ? localStorage.getItem('key') : [];
          let arrayOrdered = arrayReceived.split(',').map((el) => Number(el));
          let indexOfCancelItem = arrayOrdered.indexOf(idCancel)
          if (indexOfCancelItem != -1){
          arrayOrdered.splice(indexOfCancelItem,1)
          price = (couponBtn.disabled) ? Math.floor(price*70/100) : price;
          let cost = totalValue.textContent;
          cost = Number(cost) - (price * (Number(quant.textContent)));
          totalValue.textContent = Math.floor(cost);
          localStorage.setItem('key',arrayOrdered);
          ordered.removeChild(div);
        }})
      }

        const couponInput = document.querySelector("#cupon input")
        const couponBtn = document.querySelector("#cupon button")
        couponBtn.addEventListener("click",()=>{
          if (couponInput.value == 'Diya30'){
            let cost = totalValue.textContent;
            cost = (Number(cost) * (70/100));
            totalValue.textContent = Math.floor(cost);
            couponInput.disabled = true;
            couponBtn.disabled = true;
          }
        })
    }

    async function getDetails(){
      const jsonData = await fetch(url)
      const dataobjs = await jsonData.json();
      let dataArray = Object.entries(dataobjs.data);
      let data = dataArray.map((el)=>el[1]);
      let orderedData = (data.filter(dataele => arrayOrdered.includes(dataele.id)));
      showData(orderedData);
    }
    getDetails();
    
    // All your JS code for Order Page goes here
  </script>
</html>





INDEX.html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Home Page</title>
    <style>
      /* All Your css for Home page goes here  */
    </style>
  </head>

  <body>
    <div class="nav">
      <h1>Web Coffee Shop Menu</h1>
      <a href="./index.html">Home</a>
      <a href="./order.html">Ordered</a>
    </div>
    <div id="filter">
      <input type="number" id="lower" value="0"/>
      <input type="number" id="upper" value="1000"/>
      <button id="filter-btn">Filter</button>
    </div>
    <select id="sort">
      <option value="">Sort By Price</option>
      <option value="asc">Ascending</option>
      <option value="desc">Descending</option>
    </select>
    <h1 id="alert"></h1>
    <div id="menu-container">
      <!-- Here Append All the Items  -->
    </div>
  </body>
  <script>
    // All your JS code for Home Page goes here
  const url = `https://dbioz2ek0e.execute-api.ap-south-1.amazonaws.com/mockapi/get-coffee`;
  const ascurl = `https://dbioz2ek0e.execute-api.ap-south-1.amazonaws.com/mockapi/get-coffee?sort=price&order=asc`;
  const descurl = `https://dbioz2ek0e.execute-api.ap-south-1.amazonaws.com/mockapi/get-coffee?sort=price&order=desc`;

  const filterBtn = document.querySelector("#filter-btn")
  const menu = document.querySelector("#menu-container");
  const sortBtn = document.querySelector("#sort")
  
  

  let showData = (data) => {
    const div = document.createElement("div")
    for (let i = 0; i < data.length; i++){
      const dataEl = data[i];
      const image = document.createElement("img");
      image.setAttribute("src",dataEl.image);
      image.setAttribute("alt","image");
      const title = document.createElement("h3");
      title.textContent = dataEl.title;
      const ingredientList = document.createElement("ul");
      const ingredientListData = dataEl.ingredients;
      for (let i = 0; i < ingredientListData.length; i++){
        const ingredients = document.createElement("li")
        ingredients.innerText = ingredientListData[i];
        ingredientList.appendChild(ingredients);
      }
      const description = document.createElement("p");
      description.textContent = dataEl.description;
      const price = document.createElement("p")
      price.textContent = dataEl.price;
      const buyBtn = document.createElement("button")
      buyBtn.innerText = "Buy";
      buyBtn.classList.add("buy");
      buyBtn.setAttribute('id',`${dataEl.id}`);
      const brk = document.createElement("br");
      div.appendChild(brk);
      div.appendChild(image)
      div.appendChild(title);
      div.appendChild(ingredientList);
      div.appendChild(description);
      div.appendChild(price);
      div.appendChild(buyBtn);
    }
    menu.appendChild(div); 

    filterBtn.addEventListener("click",() => {
      const lowerinp = document.querySelector("#lower");
      const lower = lowerinp.value;
      const upperinp = document.querySelector("#upper");
      const upper = upperinp.value;
      let filteredObjs = (data.filter(dataele => ((dataele.price > lower) && (dataele.price < upper))));
      menu.innerHTML = ""
      showData(filteredObjs);
    });

    const alertHead = document.querySelector("#alert")
    const buyBtns = document.querySelectorAll(".buy");
    let arrayOrdered = (localStorage.getItem('key')) ? localStorage.getItem('key') : [];
    for (let i = 0 ; i < buyBtns.length; i++) {
      buyBtns[i].addEventListener("click", () =>{
        if (arrayOrdered.includes(buyBtns[i].id)) {
          alertHead.textContent = "Already Placed Order";
        } else {
          arrayOrdered.push(Number(buyBtns[i].id)); 
          alertHead.textContent = "Successfully Placed Order";
          localStorage.setItem('key',arrayOrdered);
        }
      })
    }
  }
 

  let getDetails = async () => {
    const jsonData = await fetch(url)
    const dataobjs = await jsonData.json();
    let dataArray = Object.entries(dataobjs.data);
    let data = dataArray.map((el)=>el[1]);
    menu.innerHTML = "";
    showData(data);
  }
  getDetails();
  

  let ascsort = async () => {
    const jsonData = await fetch(ascurl)
    const dataobjs = await jsonData.json();
    let dataArray = Object.entries(dataobjs.data);
    let data = dataArray.map((el)=>el[1]);
    menu.innerHTML = "";
    showData(data);
  }

  let descsort = async () => {
    const jsonData = await fetch(descurl)
    const dataobjs = await jsonData.json();
    let dataArray = Object.entries(dataobjs.data);
    let data = dataArray.map((el)=>el[1]);
    menu.innerHTML = "";
    showData(data);
  }

  sortBtn.addEventListener("change",() => {
    if (sortBtn.value == "asc"){
      ascsort();
    } else if (sortBtn.value == "desc"){
      descsort();
    } else {
      getDetails();
    }
  });

  </script>
</html>

