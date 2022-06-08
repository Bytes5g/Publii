<template>
    <div class="embed-consents-groups">
        <div
            v-if="content.length"
            class="embed-consents-groups-header">
            <div class="embed-consents-groups-header-cell">{{ $t('gdpr.embedConsents.groupRule') }}</div>
            <div class="embed-consents-groups-header-cell">{{ $t('gdpr.embedConsents.groupCookieGroup') }}</div>
            <div class="embed-consents-groups-header-cell">{{ $t('gdpr.embedConsents.groupButtonLabel') }}</div>
        </div>

        <div
            v-for="(group, index) of content"
            class="embed-consents-group"
            :key="'embed-consents-group-' + index">
            <text-input
                :spellcheck="false"
                v-model="group.rule"
                :placeholder="$t('gdpr.embedConsents.groupRule')" />

            <text-input
                :spellcheck="false"
                v-model="group.cookieGroup"
                :placeholder="$t('gdpr.embedConsents.groupCookieGroup')" />

            <text-input
                :spellcheck="false"
                v-model="group.buttonLabel"
                :placeholder="$t('gdpr.embedConsents.groupButtonLabel')" />

            <icon
                size="m"
                name="sidebar-close"
                @click.native="removeRule(index)" />

            <text-area
                v-model="group.text"
                :placeholder="$t('gdpr.embedConsents.groupTextPlaceholder')"
                :rows="3"></text-area>
        </div>

        <p-button
            type="small"
            @click.native="addRule">
            {{ $t('gdpr.embedConsents.addRule') }}
        </p-button>
    </div>
</template>

<script>
export default {
    name: 'embed-consents',
    props: ['value'],
    data () {
        return {
            content: []
        };
    },
    watch: {
        value (newValue) {
            this.content = newValue;
        },
        content (newValue) {
            this.$emit('input', newValue);
        }
    },
    mounted: function() {
        setTimeout(() => {
            this.content = this.value;
        }, 0);
    },
    methods: {
        addRule () {
            this.content.push({
                rule: "",
                buttonLabel: "",
                cookieGroup: "",
                text: ""
            });
        },
        removeRule (index) {
            this.content.splice(index, 1);
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../../scss/variables.scss';

.embed-consents-groups {
    border-radius: 3px;
    padding-top: 1.75rem;

    .embed-consents-groups-header {
        display: flex;
        margin-top: 1rem;

        &-cell {
            font-size: 1.4rem;
            font-weight: bold;
            margin: 0 0 1rem 0;
            width: calc((100% / 3) - 15px);
        }
    }

    .embed-consents-group {
        align-items: center;
        display: flex;
        flex-wrap: wrap;
        padding: .25rem 0;

        .input-wrapper {
            padding-right: 1rem;
            text-align: left;
            width: calc((100% / 3) - 15px);
        }

        .icon {
            cursor: pointer;
            fill: var(--warning);
            transition: all .3s ease-out;

            &:hover {
                fill: var(--icon-tertiary-color);
            }
        }

        div:last-child {
            margin-bottom: 3rem;
            margin-top: 1rem;
            width: calc(100% - 56px);
        }
    }

    .button {
        margin: 1rem 0;
    }
}
</style>