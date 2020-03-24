<script>
    import FirestoreParser from 'firestore-parser'

    let items = []
    const baseUrl = 'https://firestore.googleapis.com/v1/'
    const productsUrl = baseUrl + 'projects/svelte-native-firebase/databases/(default)/documents/products'

    const getProducts = () => {
        fetch(productsUrl)
            .then( response => response.json() )
                .then( json => FirestoreParser(json) )
                    .then( parsed => {
                        items = parsed.documents
                        console.log(items)
                     })
            .catch( error => console.log(error) )
    }

    getProducts()

</script>

<page>
    <actionBar title="Svelte Firestore REST" />
    <scrollView class="main">
        <stackLayout>
            {#each items as item}
                <flexboxLayout class="product">
                    <stackLayout>
                        <label class="h2" text={item.fields.title} />
                        <label class="body" text=${item.fields.price} />
                    </stackLayout>
                    <image src={item.fields.image} stretch="aspectFit" />
                </flexboxLayout>
            {:else}
                <activityIndicator busy={true} />
            {/each}
        </stackLayout>
    </scrollView>
</page>

<style>
    .main {
        background-color:#fff;
    }
    .product {
        margin: 20;
        padding: 15;
        background-color: #eee;
        border-radius:8;
        flex-direction: column;
        text-align: center;
    }
    .product > image {
        height:130;
    }

</style>
