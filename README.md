<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bootstrap demo</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"
      crossorigin="anonymous"
    />
  </head>
  <body>
    <div class="container mt-3">
      <form onsubmit="submitHandler(event)">
        <div class="row">
          <div class="col-6">
            <div class="mb-3">
              <label for="expenseAmount" class="form-label"
                >Choose Expense Amount</label
              >
              <input
                type="number"
                class="form-control"
                id="expenseAmount"
                name="expenseAmount"
              />
            </div>
          </div>

          <div class="col-6">
            <div class="mb-3">
              <label for="description" class="form-label"
                >Choose Description</label
              >
              <input
                type="text"
                class="form-control"
                id="description"
                name="description"
              />
            </div>
          </div>
        </div>

        <div class="row">
          <div class="col-6 mb-3">
            <select
              class="form-select"
              id="selectCategory"
              name="selectCategory"
              aria-label="Default select example"
            >
              <option selected>Choose a Category</option>
              <option value="On Vacation">On Vacation</option>
              <option value="Diesel">Diesel</option>
              <option value="Food">Food</option>
            </select>
          </div>

          <div class="col-6">
            <button type="submit" class="btn btn-primary btn-md">
              Add Expense
            </button>
          </div>
        </div>
      </form>

      <table class="table mt-5">
        <thead>
          <tr>
            <th scope="col">S.No</th>
            <th scope="col">Amount</th>
            <th scope="col">Description</th>
            <th scope="col">Category</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>

        <tbody></tbody>
      </table>
    </div>

    <script>
      function submitHandler(event) {
        event.preventDefault();

        const amount = event.target.expenseAmount.value;
        const description = event.target.description.value;
        const category = event.target.selectCategory.value;

        const entry = {
          amount: amount,
          desc: description,
          category: category,
        };

        // console.log( entry );

        localStorage.setItem(amount, JSON.stringify(entry));
        window.location.reload();
      }

      function onDelete(key) {
        // console.log("On delete " , key).
        localStorage.removeItem(key);
        window.location.reload();
      }

      function onEdit(amountt, categoryy, descc) {
        // console.log("On edit " ,  amountt , categoryy , descc )
        document.querySelector("#expenseAmount").value = amountt;
        document.querySelector("#description").value = descc;
        document.querySelector("#selectCategory").value = categoryy;

        const tdElement = document.querySelectorAll(".amount");

        tdElement.forEach((element) => {
          const domElement = element.parentElement;

          if (element.textContent === amountt) {
            // console.log(  domValue  );
            domElement.style.display = "none";
            localStorage.removeItem(amountt);
          }
        });
      }

      let localStorageLength = localStorage.length;

      // console.log( localStorageLength , localStorage.key(3)  );

      for (let i = 0; i < localStorageLength; i++) {
        const key = localStorage.key(i);

        const amount = JSON.parse(localStorage.getItem(key)).amount;
        const category = JSON.parse(localStorage.getItem(key)).category;
        const desc = JSON.parse(localStorage.getItem(key)).desc;

        const tbody = document.querySelector("tbody");

        const createTr = document.createElement("tr");
        const createSNo = document.createElement("td");
        createSNo.textContent = i + 1;

        const createTdAmount = document.createElement("td");
        createTdAmount.textContent = amount;
        createTdAmount.classList.add("amount");

        const createTdCategory = document.createElement("td");
        createTdCategory.textContent = category;

        const createTdDesc = document.createElement("td");
        createTdDesc.textContent = desc;

        // actions Create TD
        const createTdActions = document.createElement("td");
        createTdActions.innerHTML = `
            <div>
                <button class="btn btn-sm btn-primary" onclick="onEdit('${amount}' , '${category}' , '${desc}'  )" >Edit Expense </button>
                <button class="btn btn-sm btn-danger" onclick="onDelete(${amount})" >Delete Expense</button>
            </div>
            `;

        createTr.appendChild(createSNo);
        createTr.appendChild(createTdAmount);
        createTr.appendChild(createTdCategory);
        createTr.appendChild(createTdDesc);
        createTr.appendChild(createTdActions);

        tbody.appendChild(createTr);

        // console.log( amount , category , desc  );
      }
    </script>
  </body>
</html># expense-tracker
