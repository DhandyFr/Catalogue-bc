<template>
  <div class="catalog-container">
    <div class="search-bar">
      <input v-model="search" type="text" placeholder="Search product" name="search">
      <a id="search-clear-icon" @click="clearSearch">&#10006;</a>
    </div>
    <div class="product">
      <div v-for="product in productsFilter" :key="product.id" class="product-card">
        <img :src="product.primaryImage" alt="product image" class="product-image">
        <div class="product-attr">
          <div class="product-name">
            {{ product.name }}
          </div>
          <div class="product-price">
            Rp. {{ product.price }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'catalog',
  data () {
    return {
      search: '',
      products: [
        {
          id: 1,
          name: 'Kaos Polos Screamos',
          price: 50000,
          primaryImage: require('~/assets/images/kaos-polos.jpg'),
          images: [],
          color: [
            {
              name: 'Black',
              image: ''
            }
          ],
          category: 't-shirt'
        },
        {
          id: 2,
          name: 'Kaos Polos',
          price: 30000,
          primaryImage: require('~/assets/images/kaos-polos1.jpg'),
          images: [],
          color: [
            {
              name: 'Black',
              image: ''
            }
          ],
          category: 't-shirt'
        },
        {
          id: 3,
          name: 'Redknot Navi',
          price: 150000,
          primaryImage: require('~/assets/images/shoes-redknot.jpg'),
          images: [],
          color: [
            {
              name: 'Navi',
              image: ''
            }
          ],
          category: 'shoes'
        }
      ],
      productsFilter: []
    }
  },
  watch: {
    search () {
      if (this.search === '') {
        this.productInit()
      } else {
        this.searchProduct()
      }
      this.showClearIcon()
    },
    '$store.state.category' () {
      if (this.$store.state.category === '') {
        this.productInit()
      } else {
        this.filterProduct()
      }
    },
    '$store.state.minPrice' () {
      if (this.$store.state.minPrice === 0) {
        this.productInit()
      } else {
        this.filterProduct()
      }
    },
    '$store.state.maxPrice' () {
      if (this.$store.state.maxPrice === 0) {
        this.productInit()
      } else {
        this.filterProduct()
      }
    }
  },
  mounted () {
    this.productInit()
  },
  methods: {
    productInit () {
      this.productsFilter = this.products
      // this.productsFilter.forEach((prod) => {
      //   prod.price = prod.price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
      // })
    },
    searchProduct () {
      this.productsFilter = this.products.filter(prod =>
        prod.name.toLowerCase().includes(this.search.toLowerCase())
      )
    },
    showClearIcon () {
      const clearIcon = document.getElementById('search-clear-icon')
      if (this.search === '') {
        clearIcon.style.display = ''
      } else {
        clearIcon.style.display = 'flex'
      }
    },
    clearSearch () {
      this.search = ''
    },
    filterProduct () {
      const category = this.$store.state.category
      const minPrice = this.$store.state.minPrice
      const maxPrice = this.$store.state.maxPrice
      if (this.$store.state.maxPrice !== 0) {
        this.productsFilter = this.products.filter(prod =>
          prod.category.toLowerCase().includes(category.toLowerCase()) && prod.price >= minPrice && prod.price <= maxPrice
        )
      } else {
        this.productsFilter = this.products.filter(prod =>
          prod.category.toLowerCase().includes(category.toLowerCase()) && prod.price >= minPrice
        )
      }
    }
  }
}
</script>

<style>
.catalog-container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}

.search-bar {
  display: flex;
  align-items: center;
  width: 100%;
  margin: 0 0 30px 0;
  /* border: 1px solid green; */
}

.search-bar input {
  width: 100%;
  padding: 5px 10px;
  margin-right: 10px;
  font-size: 15px;
  font-family: 'Quicksand', sans-serif;
  border: 1px solid lightgrey;
  border-radius: 20px;
}

.search-bar input:focus{
  outline: none;
}

.search-bar i {
  font-size: 15px;
  background-image: url('~assets/icons/search.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 1px solid lightgrey;
  /* border-radius: 0 5px 5px 0; */
  background-size: 20px 20px;
}

#search-clear-icon {
  display: none;
  cursor: pointer;
  width: auto;
  padding: 3px 6px;
  color: rgba(0, 0, 0, .5);
  font-weight: bold;
  font-size: 14px;
  transition: 0.6s ease;
  border-radius: 50px;
  -moz-user-select: none;
  -webkit-user-select: none;
  background-color: rgba(92, 64, 80, .1);
}

#search-clear-icon:hover {
  background-color: rgba(92, 64, 80, .3);
}

.product {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: left;
}

.product-card {
  width: 180px;
  height: 300px;
  margin: 6px;
  padding: 0;
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.2);
  cursor: pointer;
  /* border: 1px solid grey; */
}

.product-card:hover {
  box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.5);
}

.product-card .product-image {
  width: 100%;
  height: 170px;
  margin: 0;
  padding: 0;
  border-radius: 10px 10px 0 0;
}

.product-card .product-attr {
  margin: 0;
  padding: 10px;
}

.product-attr .product-name {
  margin: 0 0 10px 0;
  font-family: 'Quicksand-SemiBold', sans-serif;
  color: darkslategrey;
  font-size: 17px;
}

.product-attr .product-price {
  font-family: 'Quicksand-Bold', sans-serif;
  color: #FA591D;
  font-size: 18px;
}
</style>
