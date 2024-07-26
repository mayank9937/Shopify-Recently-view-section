# Shopify Recently view section
- After adding above code in to theme, add below code into ``theme.liquid`` file.

```liquid
{%- if request.page_type == 'product' -%}
  <script>
    // We save the product ID in local storage to be eventually used for recently viewed section
    try {
      const items = JSON.parse(localStorage.getItem('theme:recently-viewed-products') || '[]');
      
      // We check if the current product already exists, and if it does not, we add it at the start
      if (items.includes({{ product.id | json }})) {
        items.splice(items.indexOf({{ product.id | json }}), 1)
      }
      items.unshift({{ product.id | json }});
      
      localStorage.setItem('theme:recently-viewed-products', JSON.stringify(items.slice(0, 20)));
      
      let recentViewedProductWrapper = document.querySelector('.js-recently-viewed-product')
      if (recentViewedProductWrapper) {
        items.length === 1 ? recentViewedProductWrapper.classList.add('hidden') : recentViewedProductWrapper.classList.remove('hidden');
      }
      
    } catch (e) {
      // Safari in private mode does not allow setting item, we silently fail
    }
  </script>
{%- endif -%}
```
