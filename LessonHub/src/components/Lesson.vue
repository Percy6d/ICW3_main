<script>
export default {
    props:['sortedLessons'],
    methods:{
        canAddToCart(lesson){
            this.$emit('canAddToCart',lesson)
        }
    }
}
</script>

<template>
         <div v-for="(lessons, index) in sortedLessons" :key="`lesson--${index}`" class="lessons__item col-md-3 py-2">
                        <img class="card-img-top" v-bind:src="lessons.img" width="200" height="250" >
                        <div class="card-body">       
                        
                            
                            <h2 class="card-title"><p>Subject: {{ lessons.subject }}</p></h2>
                            
                            <p class="card-text">Location: {{ lessons.location }}</p>
                            <p class="card-text">Price: {{ lessons.price }}</p>
                            <div>
                            
                            <p class="card-text">Spaces: {{lessons.space}}</p>
                            <button class="btn btn-dark" v-on:click='canAddToCart(lessons)' :disabled="!lessons.space">Add to
                                cart</button>
                            <span v-if="lessons.space">{{'Left '+ lessons.space}}</span>
                            <span v-else>No lessons left</span>
                        </div>
                    </div>
         </div>

</template>