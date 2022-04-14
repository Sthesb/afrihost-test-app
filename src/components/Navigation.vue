<template>
    <header :class="{ 'scrolled-nav': scrollPosition}" >
        <nav>
            <div class="icon" v-show="mobile">
                <i @click="toggleMobileNav"  class="fa fa-bars" aria-hidden="true" :class="{ 'icon-active': mobileNav }"></i>
            </div>
            <div class="branding">
                <h4>Vehicles Dashboard</h4>
            </div>
            <ul class="navigation" v-show="!mobile">
                <li><i class="fa fa-th-large" aria-hidden="true"></i><router-link class="link" :to="{name: 'home'}" >Home</router-link></li> 
                <li><i class="fa fa-line-chart" aria-hidden="true"></i><router-link class="link" :to="{name: 'about'}" >Stats</router-link></li> 
                <li><i class="fa fa-car" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Vehicles</router-link></li> 
                <li><i class="fa fa-sliders" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Config</router-link></li> 
                <li><i class="fa fa-map" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Maps</router-link></li> 
                <li><i class="fa fa-envelope" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Notification</router-link></li> 
                <li><i class="fa fa-cog" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Settings</router-link></li> 
            </ul>
            <div class="icon" v-show="mobile">
                <i   class="fa fa-bell-o" aria-hidden="true" ></i>
            </div>
            <transition name="mobile-nav">
                
                <ul class="dropdown-nav" v-show="mobileNav">
                    <i class="fa fa-times close" @click="toggleMobileNav" aria-hidden="true"></i>
                    <div class="profile">
                        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTIJF7LAdiF7JlRs24nLsBKz7nWamkcdXPODQ&usqp=CAU" alt="profile" >
                        <div class="profile_info">
                            <h4>Welcome</h4>
                            <p>John Doe</p>
                        </div>
                    </div>
                    <li><i class="fa fa-th-large" aria-hidden="true"></i><router-link class="link" :to="{name: 'home'}" >Home</router-link></li> 
                    <li><i class="fa fa-bar-chart" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Stats</router-link></li> 
                    <li><i class="fa fa-car" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Vehicles</router-link></li> 
                    <li><i class="fa fa-sliders" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Config</router-link></li> 
                    <li><i class="fa fa-map-o" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Maps</router-link></li> 
                    <li><i class="fa fa-envelope-o" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Notification</router-link></li> 
                    <li><i class="fa fa-cog" aria-hidden="true"></i><router-link class="link" :to="{name: ''}" >Settings</router-link></li>
                </ul>
            </transition>
        </nav>
    </header>
</template>
<script>
export default {
    name: 'Navigation',
    data(){
        return {
            scrollPosition: null,
            mobile: null,
            mobileNav: null,
            windowWidth: null
        }
    },
    created(){
        window.addEventListener('resize', this.checkScreen)
        this.checkScreen()
    },
    methods: {
        toggleMobileNav() {
            this.mobileNav = !this.mobileNav;
        },
        checkScreen(){
            this.windowWidth = window.innerWidth

            if(this.windowWidth <= 750){
                this.mobile = true;
                return;
            }

            this.mobile = false;
            this.mobileNav = false;

            return
        }
    }
}
</script>

<style lang="scss" scoped>
    header {
        background-color: white;
        z-index: 99;
        width: 100%;
        // position: fixed;
        transition: .5s ease all;
        color: rgba(0,0,0,0.8);

        nav {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            padding: 12px 0;
            transition: .5s ease all;
            width: 90%;
            margin: 0 auto;

            @media (min-width: 1140px) {
                max-width: 1140px
            }

            .navigation {
                display: flex;
                align-items: center;
                justify-content: flex-end;
            }
            .navigation, link {
                font-weight: 500;
                color: rgba(0,0,0,0.8);
                list-style: none;
                text-decoration: none;
            }

            li {
                text-transform: uppercase;
                margin-left: 5px;
                padding: 10px;
            }


            .link {

                font-size: 14px;
                text-decoration: none;
                transition: .5s ease all;
                margin-left: 5px;
                padding-bottom: 4px;
                border-bottom: 1px solid transparent;
                color: #898989 !important;

                &:hover {
                    color: #00afea;
                    border-color: #00afea;
                }
            }

            .branding {
                display: flex;
                align-items: center;

                h4{
                    font-size: 20px;
                }
            }
            
            .icon-active {
                transform: 180deg;
            }

            // mobile
            .dropdown-nav {
                // padding-top: 80px;
                padding-left: 20px;
                display: flex;
                flex-direction: column;
                position: fixed;
                width: 100%;
                z-index: 1;
                max-width: 250px;
                height: 100%;
                background-color: white;
                top: 0;
                left: 0;
                
                .close {
                    padding-top: 15px ;
                    padding-right: 15px ;
                    top: 0;
                    text-align: right;
                    right: 0;
                    color: red;
                    font-size: 20px;
                }

                li {
                    margin-left: 0;
                    list-style: none;
                    text-decoration: none;
                    border-left: 2px solid transparent;
                    

                    &:hover {
                        background-color: #55d4ff;
                        border-color: rgb(75, 75, 255);
                        transition: .5s ease all;
                        color: #00afea;
                    }

                    .link {
                        color: rgba(0,0,0,0.8);
                        &:hover {
                            color: #00afea;
                        }
                    }
                }

                .profile {
                    margin-top: 60px;
                    background-color: white;
                    margin-bottom: 15px;
                    display: flex;
                    justify-items: center;
                    align-items: center;
                    img {
                        height: 50px;
                        border-radius: 10px;
                        object-fit: contain;
                    }

                    .profile_info {
                        padding-left: 10px;
                    }
                }

            }
        }
    }
</style>