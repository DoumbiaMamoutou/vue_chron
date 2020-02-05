# vue_chron
utilisation cron job avec vue js



```html
<script>
    app = new Vue({
        el:'#app',
        data: {
            presence : [],
            datarefresh: [],
            job:'',
            loader:true,
            success:false,
            error:false,
            error_message:'',
            base_url: window.location.protocol + "//" + window.location.host,
            refresh_url: '/apiPresence/?format=json'
        },
        mounted(){
            // Run a function each second
            this.job = Cron('*/5 * * * * *', this.refreshdata)
        // // Pause job
        // job.pause();

        // // Resume job
        // job.resume();

        // // Stop job
        // job.stop();
        },
        delimiters:["${","}"],
        methods: {
            get_machine:function(machine){
                if(machine){
                    return machine.site.nom
                } else {
                    return null
                }
            },
            refreshdata:function(){
                axios.defaults.xsrfCookieName = 'csrftoken';
                axios.defaults.xsrfHeaderName = 'X-CSRFToken';
                axios.get(this.base_url + this.refresh_url)
                    .then((response) => {
                    this.presence = response.data;
                    this.isregister=false;
                    this.loader=false
                }).catch((err) => {
                    console.log(err)

                })
            },
        }
    })

</script>
```
