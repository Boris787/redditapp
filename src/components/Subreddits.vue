    <template>
        <div class="subreddits container">
            <h2>{{category | capitalize}}</h2>
            <ul class="item-list">
                <li v-for="post in subredditsData">
                    <subreddit :item="post"></subreddit>
                </li>
            </ul>
        </div>  
    </template>

    <script>
        import Subreddit from './Subreddit';
        import axios from 'axios';

        export default {
            name: 'subreddits',
            props: ['category'],
            data() {
                return {
                    subredditsData: []
                }
            },
            components : {
                Subreddit // Import components by giving them the same name as imported
            },
            filters: {
                capitalize: function(value) {
                    if(!value) return '';
                    value = value.toString();
                    return value.charAt(0).toUpperCase() + value.slice(1);
                }
            },
            mounted() {
                this.fetchData();
            },
            methods: {
                fetchData() {
                    axios({ url: `https://www.reddit.com/r/${this.category}/top.json?limit=5`, method: 'GET' }).then(response => {
                        this.subredditsData = response.data.data.children;
                    });
                }
            }
        }
    </script>

    <style scoped>
        .container {
            max-width: 600px;
            margin: 30px auto;
            background: #ffffff;
            box-shadow: 0 0 3px #cccccc;
        }

        .subreddits {
            min-width: 400px;
            padding: 25px 45px;
        }

        .subreddits h2 {
            font-size: 20px;
            margin-bottom: 10px;
            margin-top: 0;
        }

        .subreddits .item-list {
            border-top: 1px solid #cccccc;
            padding-top: 20px;
            list-style: none; 
        }

         .subreddits .item-list li {
             margin-bottom: 20px;
         }
    </style>