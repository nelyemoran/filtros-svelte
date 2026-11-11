<main class="container">
<h1>PRODUCTOS EN VENTA</h1>
<p>Elige tu Producto: </p>

<div class="filters">
    <input bind:value={searchProduct} placeholder="Buscar producto...">
        <select bind:value={searchCategories}>
            {#each categories as category}
              <option value={category}>{category}</option>
            {/each}
        </select>
</div>

    <div class="product-list">
        {#each filteredProduct() as product (product.id)} <!--Recorrer el array de productos-->
            <div class="card">
                <h2>{product.name}</h2>
                <p>Categoria: {product.category}</p>
                <p>Precio: ${product.price.toLocaleString('es-CO')}</p>
                {#if product.off}
                    <p class="badge">¡OFERTA!</p>
                {/if}
            </div>
            {:else}
                <p class="no-products">No se encontraron productos.</p>
            {/each}
    </div>
</main>

<script lang="ts">
//Interface para definir la estructura de un producto
interface Product {
    id: number;
    name: string;
    category: string;
    price: number;
    off?: boolean; //Propiedad opcional para indicar si el producto tiene descuento
} 
//Array de productos
let products: Product[] = [
{id: 1, name: "Manzana", category: "Frutas", price: 400},
{id: 2, name: "Banana", category: "Frutas", price: 200},
{id: 3, name: "Pera", category: "Frutas", price: 400},
{id: 4, name: "Naranja", category: "Frutas", price: 300},
{id: 5, name: "Pulpa", category: "Congelados", price: 2000},
{id: 6, name: "Zanahoria", category: "Verduras", price: 500, off: false},
{id: 7, name: "Rubor", category: "Cosmeticos", price: 12000, off: true},
{id: 8, name: "Leche", category: "Lacteos", price: 3990},
{id: 9, name: "Crema de Leche", category: "Lacteos", price: 4600, off: true},
{id: 10, name: "Yogurt", category: "Lacteos", price: 1200, off: true},
{id: 11, name: "Jugo del Valle", category: "Bebidas", price: 400},
{id: 12, name: "Jugo Nectar Manzana", category: "Bebidas", price: 2000},
{id: 13, name: "Cereal", category: "Cereales", price: 6700, off: true},
{id: 14, name: "Oreo", category: "Dulces", price: 3200},
{id: 15, name: "Festival", category: "Dulces", price: 3200, off: true},
{id: 16, name: "Chicles", category: "Dulces", price: 3200},
{id: 17, name: "Zapatos", category: "Calzados", price: 45000, off: true},
{id: 18, name: "Camisa", category: "Ropa", price: 25000, off: true}
];

//Buscador 
let searchProduct= $state('');
let searchCategories= $state('Todas'); //selecionar categoria

//devuelve producto de buscador
let filteredProduct = () => {
    return products.filter(product => {
        const matchesProduct = product.name.toLocaleLowerCase().includes(searchProduct.toLocaleLowerCase());
        const matchesCategory = searchCategories === 'Todas' || product.category === searchCategories;
        return matchesProduct && matchesCategory;
    });
}

//lista de las categorias a mostrar en el select
let categories= ["Todas", ...new Set(products.map(product => product.category))]; //Set para obtener categorias unicas
</script>

<style>
    .container {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
    }

    h1 {
        color: #1c7c42;
        text-align: center;
        margin-bottom: 20px;
    }

    .filters {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        margin-bottom: 20px;
    }

    input, select {
        padding: 10px;
        font-size: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    input{
        flex: 1;
    }

    input:focus,
    select:focus {
        border-color: #1c7c42;
        border-radius: 4px;
    }

    .product-list {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
        gap: 20px;
    }

    .card {
        background-color: #88e0b6;
        border: 1px solid #ccc;
        border-radius: 8px;
        padding: 20px;
        text-align: center;
        transition: transform 0.3s, background-color 0.3s, color 0.3s;
    }
        .card:hover {
            background-color: #1c7c42;
            color: #fff;
            transform: translateY(-5px);
        }

    .badge {
        background-color: #fc0707;
        color: rgb(255, 255, 255);
        font-weight: bold;
        display: inline-block;
        padding: 5px 10px;
        border-radius: 4px;
    }

    .no-products {
        grid-column: 1 / -1;
        text-align: center;
        color: #666;
    }
</style>