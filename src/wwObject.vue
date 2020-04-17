<template>
    <div class="ww-share-wrapper">
        <div class="ww-share" @click="share">
            <!-- wwManager:start -->
            <wwOrangeButton class="ww-orange-button" v-if="wwObjectCtrl.getSectionCtrl().getEditMode() == 'CONTENT'"></wwOrangeButton>
            <!-- wwManager:end -->
            <wwObject :ww-object="wwObject.content.data.icon" ww-inside-ww-object="ww-share" ww-default-object-type="ww-icon" ww-object-types-allowed="['ww-icon', 'ww-button', 'ww-text']" :ww-no-section="wwAttrs.wwNoSection" :ww-no-link="wwAttrs.wwNoLink" ww-force-edit-mode="CONTENT"></wwObject>
        </div>
    </div>
</template>

<script>
/* wwManager:start */
import wwSharePopup from './wwSharePopup.vue';
wwLib.wwPopups.addPopup('wwSharePopup', wwSharePopup);
/* wwManager:end */

export default {
    name: 'ww-share',
    props: {
        wwObjectCtrl: Object,
        wwAttrs: {
            type: Object,
            default: {}
        }
    },
    data() {
        return {};
    },
    computed: {
        wwObject() {
            return this.wwObjectCtrl.get();
        },
    },
    methods: {
        init() {
            let needUpdate = false;
            this.wwObject.content.data = this.wwObject.content.data || {};

            if (!this.wwObject.content.data.icon) {
                this.wwObject.content.data.icon = wwLib.wwObject.getDefault({ type: 'ww-icon', data: { icon: 'fas fa-share' } });
                needUpdate = true;
            }

            if (needUpdate) {
                this.wwObjectCtrl.update(this.wwObject);
            }
        },
        share() {
            if (this.wwObjectCtrl.getSectionCtrl().getEditMode() == 'CONTENT') return 
            let url = this.wwObject.content.data.socialNet.url
            url = url.replace('{url}', window.location.href)
            window.open(url, '_blank')
        },
        /* wwManager:start */
        async edit() {
            wwLib.wwPopups.addStory('WWSHARE_SOCIAL_NET', {
                title: {
                    en: 'Share button social network',
                    fr: 'Bouton de partage sur réseau social',
                },
                type: 'wwSharePopup',
                buttons: {
                    OK: {
                        text: {
                            en: 'Save',
                            fr: 'Enregistrer',
                        },
                        next: false,
                    },
                },
            });

            wwLib.wwPopups.addStory('WWSHARE_EDIT', {
                title: {
                    en: 'Edit share button',
                    fr: 'Editer le bouton de partage',
                },
                type: 'wwPopupEditWwObject',
                storyData: {
                    list: {
                        EDIT_SOCIAL_NET: {
                            separator: {
                                en: 'Link',
                                fr: 'Lien',
                            },
                            title: {
                                en: 'Change social network',
                                fr: 'Changer le réseau social',
                            },
                            desc: {
                                en: 'Facebook, twitter, linkedIn ...',
                                fr: 'Facebook, twitter, linkedIn ...',
                            },
                            icon: 'fas fa-share',
                            shortcut: 's',
                            next: 'WWSHARE_SOCIAL_NET',
                        },
                    },
                },
            });

            const options = {
                firstPage: 'WWSHARE_EDIT',
                data: {
                    wwObject: this.wwObject,
                },
            };
            this.wwObjectCtrl.update(this.wwObject);
            try {
                const result = await wwLib.wwPopups.open(options);
                console.log(result)
                /*=============================================m_ÔÔ_m=============================================\
                  SOCIAL NET
                \================================================================================================*/
                if (typeof result.socialNet != 'undefined') {
                    this.wwObject.content.data.socialNet = result.socialNet;
                }
                this.wwObjectCtrl.update(this.wwObject);
                this.wwObjectCtrl.globalEdit(result);
            } catch (error) {
                wwLib.wwLog.log(error);
            }
        },
        /* wwManager:end */
    },
    mounted() {
        this.init();
        this.$emit('ww-loaded', this);
    },
};
</script>

<style scoped>
.ww-share-wrapper {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: initial;
    line-height: initial;
    color: initial;
    user-select: none;
}
.ww-share {
    display: inline-block;
    position: relative;
    cursor: pointer;
}
/* wwManager:start */
.ww-orange-button {
    position: absolute;
    top: 0;
    left: 0;
    transform: translate(-50%, -50%);
    z-index: 1;
}
/* wwManager:end */
</style>
