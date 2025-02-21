<template>
    <div id="component"
        v-on:click="update()">
        <div class="columns chooser-icon-column is-gapless is-inline-flex">
            <div class="column is-2-mobile is-2" id="icon-col">
                <input
                    :id="id"
                    type="checkbox"
                    class="css-checkbox"
                    ref="input">
                <label
                    :for="id"
                    :class="icon">
                </label>
            </div>
            <div class="column icon-description is-9-mobile is-10">
                <b ref="heading">heading</b>
                <p>{{description}}</p>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'IconSelector',
    props: ['id', 'icon', 'value'],
    methods: {
        update() {
            var toggle = this.$refs.input
            toggle.checked = !toggle.checked

            this.$emit('input', this.$props.id === 'sa' ? toggle.checked : !toggle.checked)

            const clickOrTap = screen.width <= 768 ? 'Tap to ' : 'Click to '
            this.$refs.heading.innerHTML =
                clickOrTap + (toggle.checked
                    ? this.heading_allow
                    : this.heading_disallow)
        }
    },
    computed: {
        description() {
            switch (this.icon) {
            case 'nc': return 'You let others copy, distribute, display, perform, and (unless you have chosen NoDerivatives) modify and use your work for any purpose other than commercially, unless they get your permission first.'
            case 'nd': return 'You let others copy, distribute, display, and perform, and modify your work, as long as they distribute any modified work, they must get your permission first.'
            case 'sa': return 'You let others copy, distribute, perform, and modify your work, as long as they distribute any modified work on the same terms. If they want to distribute modified works under other terms, they must get your permission first.'
            default: break
            }
        },
        heading_disallow() {
            switch (this.icon) {
            case 'nd': return 'Disallow Modifications of Your Work'
            case 'nc': return 'Disallow Commercial Uses of Your Work'
            case 'sa': return 'Require Disribution on Same Terms'
            default: return null
            }
        },
        heading_allow() {
            switch (this.icon) {
            case 'nd': return 'Allow Others to Modify Your Work'
            case 'nc': return 'Allow Commercial Uses of Your Work'
            case 'sa': return 'Not Require Distribution on Same Terms'
            default: return null
            }
        },
        imageSrc() {
            switch (this.icon) {
            /* eslint-disable */
            case 'nd': return "../assets/license-icons/cc-nd-icon.svg"
            case 'nc': return "../assets/license-icons/cc-nc-icon.svg"
            case 'sa': return "../assets/license-icons/cc-sa-icon.svg"
            default: break
            /* eslint-enable */
            }
        }
    },
    mounted: function() {
        // When the ND selector is toggled, the SA selector instance is
        // destroyed, and its state is reset. This reset is not reflected
        // in the chooser component instance. This method sets the proper
        // selector state on mount
        if (this.$props.id === 'sa' && this.$props.value) {
            this.$refs.input.checked = true
            this.$refs.heading.innerHTML = 'Click to ' + this.heading_allow
        } else {
            this.$refs.heading.innerHTML = 'Click to ' + this.heading_disallow
        }
    }
}
</script>
<style scoped>
    #component {
        cursor: pointer;
        margin-top: 2%;
        margin-bottom: 1.5%;
        user-select: none;
        -webkit-user-select: none;
        -ms-user-select: none;
    }
    .icon-description {
        vertical-align: middle
    }
    input[type=checkbox].css-checkbox{ display: none; }
    label {
        display: flex;
        height: 65px;
        padding: 20px 50px 40px 20px;
        margin-top: 25%;
        background-repeat: no-repeat;
        background-size: contain;
        cursor: pointer;
        opacity: 0.4;
        filter: alpha(opacity=40); /* msie */
    }
    .css-checkbox:checked + label {
        background-position: 0 0;
        opacity: 1;
        filter: alpha(opacity=100); /* msie */
    }
    .css-checkbox:hover + label {
        opacity: .5;
        filter: alpha(opacity=50);
    }
    label.nd {
        background-image: url(../assets/license-icons/cc-nd-icon.svg);
    }
    input[type=checkbox].css-checkbox:checked + label.nd {
        background-image: url(../assets/license-icons/cc-nd-icon.svg);
        opacity: 1;
        filter: alpha(opacity=100); /* msie */
    }
    label.nc {
        background-image: url(../assets/license-icons/cc-nc_icon.svg)
    }
    input[type=checkbox].css-checkbox:checked + label.nc {
        background-image: url(../assets/license-icons/cc-nc_icon.svg);
        opacity: 1;
        filter: alpha(opacity=100); /* msie */
    }
    label.sa {
        background-image: url(../assets/license-icons/cc-sa_icon.svg);
    }
    input[type=checkbox].css-checkbox:checked + label.sa {
        background-image: url(../assets/license-icons/cc-sa_icon.svg);
        opacity: 1;
        filter: alpha(opacity=100); /* msie */
    }

    @media only screen and (max-width: 670px) {
        label {
            height: 55px !important;
            width: auto !important;
            padding: 0;
            margin-top: 0px;
        }

        b, p {
            font-size: 14px;
        }

        #icon-col {
            margin-right: 6px;
        }
    }
</style>
