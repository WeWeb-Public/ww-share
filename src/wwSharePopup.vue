<template>
    <div class="ww-popup-style">
        <div class="content">
            <div class="providers">
                <wwManagerTitle>Default Social Network</wwManagerTitle>
                <div class="provider" v-for="provider in providers" :key="provider.name">
                    <wwManagerRadio :value="props.socialNet.name === provider.name" @change="selectProvider(provider)"></wwManagerRadio>
                    <i class="icon" :class="[provider.icon, provider.name]"></i>
                    <div class="name">{{ provider.name }}</div>
                </div>
            </div>
            <div class="providers-custom">
                <wwManagerTitle>Custom Social Network</wwManagerTitle>
                <wwManagerInput class="input" color="orange" :value="props.socialNet.url" @change="customCustomProvider($event)" label="Url"></wwManagerInput>
                <em>The url must contain the key {url}. It will be replace by the current page url.</em>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'wwSharePopup',
    props: {
        options: {
            type: Object,
            default: function () {
                return {};
            },
        },
    },
    data() {
        return {
            providers: [
                {
                    name: 'Facebook',
                    icon: 'fab fa-facebook-square',
                    url: 'https://www.facebook.com/sharer.php?u={url}',
                },
                {
                    name: 'Twitter',
                    icon: 'fab fa-twitter-square',
                    url: 'https://twitter.com/share?url={url}',
                },
                {
                    name: 'LinkedIn',
                    icon: 'fab fa-linkedin',
                    url: 'https://www.linkedin.com/shareArticle?mini=true&url={url}',
                },
                {
                    name: 'Pinterest',
                    icon: 'fab fa-pinterest',
                    url: 'http://pinterest.com/pin/create/link/?url={url}',
                },
                {
                    name: 'Reddit',
                    icon: 'fab fa-reddit',
                    url: 'https://reddit.com/submit?url={url}',
                },
                {
                    name: 'Tumblr',
                    icon: 'fab fa-tumblr',
                    url: 'https://www.tumblr.com/widgets/share/tool?canonicalUrl={url}',
                },
            ],
            wwObject: this.options.data.wwObject,
            /*=============================================m_ÔÔ_m=============================================\
              PROPS
            \================================================================================================*/
            props: {
                socialNet: {},
            },
        };
    },
    methods: {
        init() {
            this.props.socialNet = this.wwObject.content.data.socialNet || {};
        },
        setResult() {
            for (const key in this.props) {
                this.options.result[key] = this.props[key];
            }
        },
        selectProvider(provider) {
            this.props.socialNet = JSON.parse(JSON.stringify(provider));
            this.setResult();
        },
        customCustomProvider(url) {
            this.props.socialNet.name = undefined;
            this.props.socialNet.url = url;
            this.setResult();
        }
    },
    mounted() {
        this.init();
    },
};
</script>

<style scoped lang="scss">
.ww-popup-style {
    .content {
        display: flex;
        flex-wrap: wrap;
        width: 90%;
        margin: 10px auto;
        @media (min-width: 1024px) {
            width: 40%;
        }
        .elem {
            margin: 10px auto;
            width: 100%;
            .input {
                width: 100%;
            }
            .button {
                width: fit-content;
            }
        }
        .providers {
            .provider {
                display: flex;
                align-items: center;
                margin: 15px;
                .icon {
                    font-size: 1.5rem;
                    padding: 0 10px;
                    width: 40px;
                    text-align: center;
                    &.Facebook {
                        color: #3b5998;
                    }
                    &.Twitter {
                        color: #1da1f2;
                    }
                    &.LinkedIn {
                        color: #2867b2;
                    }
                    &.Pinterest {
                        color: #BD081C;
                    }
                    &.Reddit {
                        color: #ff4500;
                    }
                    &.Tumblr {
                        color: #001935;
                    }
                }
                .name {
                    font-size: 1.2rem;
                }
            }
        }
        .providers-custom {
            margin-top: 20px;
            width: 100%;
            .input {
                width: 100%;
            }
        }
    }
}
</style>
