/***********************
 * ESTADO GLOBAL (NO TOCAR)
 ***********************/
const state = {
  users: [],
  menuItems: [],
  orders: [],
  currentUser: null,
  currentOrder: []
};

/***********************
 * ZONA 1️⃣ USUARIOS (CAMBIABLE)
 * 👉 Aquí cambias roles, nombres, emails
 ***********************/
function initializeUsers() {
  state.users = [
    {
      id: 1,
      name: "Admin",
      email: "admin@restorapp.com",
      role: "admin"
    },
    {
      id: 2,
      name: "Juan",
      email: "juan@mail.com",
      role: "user"
    }
  ];
}

/***********************
 * ZONA 2️⃣ DATOS DEL TEMA (CAMBIABLE)
 * 👉 AQUÍ CAMBIAS EL NEGOCIO COMPLETO
 ***********************/
function initializeMenu() {
  state.menuItems = [
    {
      id: 1,
      name: "Hamburguesa Clásica",
      price: 12000,
      category: "Comida"
    },
    {
      id: 2,
      name: "Papas Fritas",
      price: 6000,
      category: "Acompañante"
    },
    {
      id: 3,
      name: "Gaseosa",
      price: 4000,
      category: "Bebida"
    }
  ];
}

/*
👉 EJEMPLOS DE CAMBIO DE TEMA:

TIENDA:
- name: "Mouse Gamer"
- price: 80000
- category: "Electrónica"

CINE:
- name: "Entrada General"
- price: 15000
- category: "Boleta"

FARMACIA:
- name: "Acetaminofén"
- price: 5000
- category: "Medicamento"
*/

/***********************
 * LOGIN (NO TOCAR)
 ***********************/
function login(email) {
  const user = state.users.find(u => u.email === email);
  if (!user) {
    alert("Usuario no encontrado");
    return;
  }

  state.currentUser = user;
  saveSession();
  renderView();
}

/***********************
 * CONTROL DE VISTAS (NO TOCAR)
 ***********************/
function renderView() {
  hideAllViews();

  if (!state.currentUser) {
    loginView.classList.remove("hidden");
    return;
  }

  if (state.currentUser.role === "admin") {
    adminView.classList.remove("hidden");
    renderAdminOrders();
  } else {
    userView.classList.remove("hidden");
    renderMenu();
    renderOrderSummary();
  }
}

function hideAllViews() {
  loginView.classList.add("hidden");
  userView.classList.add("hidden");
  adminView.classList.add("hidden");
  profileView.classList.add("hidden");
}

/***********************
 * MENÚ / CATÁLOGO (NO TOCAR)
 ***********************/
function renderMenu() {
  menu.innerHTML = "";

  state.menuItems.forEach(item => {
    const div = document.createElement("div");
    div.innerHTML = `
      <h4>${item.name}</h4>
      <p>$${item.price}</p>
      <button data-id="${item.id}">Agregar</button>
    `;

    div.querySelector("button").addEventListener("click", () => {
      addToOrder(item.id);
    });

    menu.appendChild(div);
  });
}

/***********************
 * PEDIDO / CARRITO (NO TOCAR)
 ***********************/
function addToOrder(itemId) {
  const product = state.menuItems.find(p => p.id === itemId);
  state.currentOrder.push(product);
  renderOrderSummary();
}

function renderOrderSummary() {
  orderSummary.innerHTML = "";

  let total = 0;

  state.currentOrder.forEach(item => {
    total += item.price;
    const p = document.createElement("p");
    p.textContent = `${item.name} - $${item.price}`;
    orderSummary.appendChild(p);
  });

  const totalP = document.createElement("strong");
  totalP.textContent = `Total: $${total}`;
  orderSummary.appendChild(totalP);
}

/***********************
 * ZONA 3️⃣ ESTADOS (CAMBIABLE)
 ***********************/
const ORDER_STATUS_FLOW = [
  "pendiente",
  "preparando",
  "listo",
  "entregado"
];

/*
👉 Si cambian los estados:
- solicitud → aprobado → enviado → completado
SOLO CAMBIAS ESTE ARRAY
*/

/***********************
 * CONFIRMAR PEDIDO (NO TOCAR)
 ***********************/
function confirmOrder() {
  if (state.currentOrder.length === 0) return;

  const total = state.currentOrder.reduce((sum, i) => sum + i.price, 0);

  const order = {
    id: Date.now(),
    userId: state.currentUser.id,
    items: state.currentOrder,
    total,
    status: ORDER_STATUS_FLOW[0],
    createdAt: new Date().toISOString()
  };

  state.orders.push(order);
  state.currentOrder = [];
  saveOrders();
  renderOrderSummary();
}

/***********************
 * ADMIN PANEL (NO TOCAR)
 ***********************/
function renderAdminOrders() {
  adminOrders.innerHTML = "";

  state.orders.forEach(order => {
    const div = document.createElement("div");
    div.innerHTML = `
      <h4>Pedido #${order.id}</h4>
      <p>Total: $${order.total}</p>
      <p>Estado: ${order.status}</p>
      <button>Cambiar estado</button>
    `;

    div.querySelector("button").addEventListener("click", () => {
      updateOrderStatus(order.id);
    });

    adminOrders.appendChild(div);
  });
}

/***********************
 * CAMBIO DE ESTADO (NO TOCAR)
 ***********************/
function updateOrderStatus(orderId) {
  const order = state.orders.find(o => o.id === orderId);
  const index = ORDER_STATUS_FLOW.indexOf(order.status);

  if (index < ORDER_STATUS_FLOW.length - 1) {
    order.status = ORDER_STATUS_FLOW[index + 1];
    saveOrders();
    renderAdminOrders();
  }
}

/***********************
 * LOCALSTORAGE (NO TOCAR)
 ***********************/
function saveSession() {
  localStorage.setItem("session", JSON.stringify(state.currentUser));
}

function saveOrders() {
  localStorage.setItem("orders", JSON.stringify(state.orders));
}
