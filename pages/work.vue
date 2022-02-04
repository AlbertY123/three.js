<template>
    <div class = "bg-black h-screen px-6 py-12">
        <h2 class="text-white font-bold text-5xl opacity-0" ref="title" style = "transform: translateY(30px)">My Work</h2>
     
        <div class="flex space-x-4 w-full">
            <div ref="projects" v-for="project in projects" class="w-full opacity-0">
            <nuxt-link to="/">
                <img :src="project.image.url" 
                alt="iPhone(Tech)" 
                style = "height:70vh;" 
                class="object-cover">
            <p class="text-white font-space-mono text-sm">{{project.title}}</p>
            </nuxt-link>
        </div>
    </div>
</div>
</template>

<script>
    import gsap from 'gsap'
    import iphoneImg from"~/static/iphone.jpg"
    import laptopImg from"~/static/laptop.jpg"
export default {
    data(){
        return{
            projects: [
            {
                image:{
                    url:iphoneImg
                },
                title: 'Project'
            },
            {
                image:{
                    url:laptopImg
                },
                title: 'Earth'
            },
            {
                image:{
                    url:iphoneImg
                },
                title: 'Project'
            },
            {
                image:{
                    url:laptopImg
                },
                title: 'Earth'
            },
            {
                image:{
                    url:iphoneImg
                },
                title: 'Project'
            },
            {
                image:{
                    url:laptopImg
                },
                title: 'Earth'
            }
            ]
        }
    },
    mounted(){
        gsap.to(this.$refs.title, {
            opacity: 1,
            duration: 2,
            y:0,
            ease: 'expo'
        })
        gsap.to(this.$refs.projects, {
            opacity: 1,
            duration: 2,
            stagger: 0.1,
            x: 0,
            y:0,
            ease: 'expo',
            delay: 0.3
        })
    }
}
</script>
