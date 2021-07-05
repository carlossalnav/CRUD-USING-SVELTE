<script>
import { assign } from "svelte/internal";
import {v4} from "uuid";
import Noty from "noty";

import "noty/lib/noty.css";
import "noty/lib/themes/metroui.css"

    let products = [
        {
            id: 0,
            name: "Asus ROG Zephyrus G15 Ultra Slim Gaming Laptop",
            price: 4989,
            description: "Laptop, 15.6 240Hz Pantone Validada FHD, GeForce RTX 2060 MAX-Q, AMD Ryzen 7 4800HS, 16GB DDR4, 1TB PCIe SSD, Gig+ Wi-Fi 6, Windows 10 Pro, GA502IV-XS76",
            category: "Laptop",
            imageURL: "https://m.media-amazon.com/images/I/71ucq84m3pL._AC_SS450_.jpg"
        },
        {
            id: 1,
            name: "Logitech - MX Ergo",
            price: 1350.90,
            description: "Mouse Inalámbrico Ergonómico Multidispositivo con Bisagra Ajustable de color Gris",
            category: "Mouse",
            imageURL: "https://resource.logitech.com/w_420,h_420,c_limit,q_auto,f_auto,dpr_1.0/content/dam/logitech/en/products/mice/mx-ergo/gallery/mx-ergo-gallery-06.png?v=1"
        },
        {
            id: 2,
            name: "Realme - 7 Pro Smartphone de 6.4, 8GB RAM + 128GB ROM",
            price: 5798,
            description: "Pantalla SuperAMOLED FHD+, procesador Octa-Core Snapdragon 720G, Carga Super Dart de 60W (Azul Espejo)",
            category: "Cellphone",
            imageURL: "https://m.media-amazon.com/images/I/81UBWP+4LWL._AC_SS450_.jpg"
        },
        {
            id: 3,
            name: "Asus ROG Falchion Wireless 65%",
            price: 3676.11,
            description: "Teclado mecánico para Juegos (68 Teclas, Aura Sync RGB, batería extendida, Panel táctil Interactivo, Teclas PBT, interruptores Cherry MX Rojo, Funda para Teclado)",
            category: "Keyboard",
            imageURL: "https://dlcdnwebimgs.asus.com/gain/4DF67E2E-0C91-4FC6-AA79-7B804729BB9B/w2000/h1470"
        }

    ];

    let product = {
        id: "",
        name: "",
        price: "",
        description: "",
        category: "",
        imageURL: ""
    }

    const addProduct = () => {
        const newProduct = {
            id: v4(),
            name: product.name,
            price: product.price,
            description: product.description,
            category: product.category,
            imageURL: product.imageURL
        }
        products = products.concat(newProduct)
        console.log(products)
        cleanProduct()
        new Noty({
            theme: "metroui",
            layout:"bottomRight",
            type: "info",
            timeout : 2000,
            text: "Product Added Successfully"
        }) .show()
    }

    const updateProduct = () => {
        let updatedProduct = {
            name: product.name,
            price: product.price,
            description: product.description,
            category: product.category,
            imageURL: product.imageURL
        }
        const productIndex = products.findIndex(p => p.id === product.id)
        products[productIndex] = updatedProduct;
        cleanProduct()
        editStatus = false
        new Noty({
            theme: "metroui",
            layout:"bottomRight",
            type: "success",
            timeout : 2000,
            text: "Product Updated Successfully"
        }) .show()
    }

    const cleanProduct = () => {
        product = {
            id: "",
            name: "",
            price: "",
            description: "",
            category: "",
            imageURL: "" }
    }

    const editProduct = productEdited => {
        product = productEdited
        editStatus = true;
    }

    let editStatus = false;

    const deleteProduct = (id) => {
        products = products.filter(product => product.id !== id)
        new Noty({
            theme: "metroui",
            type: "error",
            layout:"bottomRight",
            timeout : 2000,
            text: "Product Deleted Successfully"
        }) .show()
    }

    const onSubmitHandler = e => {
        if (!editStatus) {
            addProduct()
        } else {
            updateProduct()
        }
    }

</script>

<main>
<div class="container">
    <div class="row">
        <div class="col-md-6">
            {#each products as product}
                <div class="card mt-2">
                    <div class="row">
                        <div class="col-md-4">
                        {#if !product.imageURL}
                            <img src="img/No_Product_Found.png" alt="" class="img-fluid m-3">
                        {:else}
                            <img src="{product.imageURL}" alt="" class="img-fluid m-3">
                        {/if}
                        </div>
                        <div class="col-md-8">
                            <div class="card-body text-justify">
                                <h5><strong>{product.name}</strong></h5>
                                <h4><strong>${product.price}</strong></h4>
                                <p class="card-text">{product.description}</p>
                                <p><small>Category: {product.category}</small></p>
                                <button class="btn btn-danger" on:click={deleteProduct(product.id)}>
                                    Delete
                                </button>
                                <button class="btn btn-secondary" on:click="{editProduct(product)}">
                                    Edit
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            {/each}
        </div>
        <div class="col-md-6">
            <div class="card mt-2">
                <div class="card-body">
                    <form on:submit|preventDefault={onSubmitHandler} class="">
                        <div class="form-group">
                            <input bind:value={product.name} type="text" placeholder="Product Name" id="product-name" class="form-control">
                        </div>
                        <div class="form-group">
                            <input bind:value={product.price} type="number"  step="0.01" placeholder="Product Price" id="product-price" class="form-control">
                        </div>
                        <div class="form-group">
                            <textarea bind:value={product.description} id="product-description" rows="3" placeholder="Product Description" class="form-control"></textarea>
                        </div>
                        <div class="form-group">
                            <input bind:value={product.imageURL} type="url" id="product-image-url" placeholder="Image URL" class="form-control">
                        </div>
                        <div class="form-group">
                            <select bind:value={product.category} id="category" class="form-control">
                                <option value="Laptop">Laptop</option>
                                <option value="Cellphone">Cellphone</option>
                                <option value="Mouse">Mouse</option>
                                <option value="Keyboard">Keyboard</option>
                            </select>
                        </div>
                    <button class="btn btn-secondary">
                        {#if !editStatus}
                            Save Product
                        {:else}
                            Update Product
                        {/if}
                    </button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
<br> <br>
</main>

<style>
</style>