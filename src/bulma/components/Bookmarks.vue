<template>
    <core-bookmarks>
        <template v-slot:default="{
                bookmarks, stickies, reorderBindings, reorderEvents,
                remove, stick, clear, isExcluded
            }">
            <div class="bookmarks">
                <span class="control">
                    <a v-if="stickies.length"
                        class="tag is-warning icon has-margin-right-small"
                        @click="clear">
                        <fa icon="trash-alt"/>
                    </a>
                </span>
                <draggable class="field is-grouped bookmark-items no-scrollbars"
                    v-bind="reorderBindings"
                    v-on="reorderEvents">
                    <span v-for="bookmark in bookmarks"
                        :key="bookmark.name"
                        class="control">
                        <span class="tags has-addons">
                            <a :class="['tag is-bold', {'is-link': bookmark.name === $route.name}]"
                                @click="$router.push({
                                    name: bookmark.name,
                                    params: bookmark.params,
                                    query: bookmark.query
                                })">
                                {{ i18n(bookmark.meta.title) }}
                            </a>
                            <a v-if="!bookmark.sticky && !isExcluded(bookmark)"
                                class="tag is-success check"
                                @click="stick(bookmark)">
                                <span class="icon is-small">
                                    <fa icon="check"/>
                                </span>
                            </a>
                            <a v-if="bookmarks.length > 1 && !isExcluded(bookmark)"
                                class="tag is-delete"
                                @click="remove(bookmark)"/>
                        </span>
                    </span>
                </draggable>
            </div>
        </template>
    </core-bookmarks>
</template>

<script>
import Draggable from 'vuedraggable';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faCheck, faTrashAlt } from '@fortawesome/free-solid-svg-icons';
import CoreBookmarks from '../../core/components/Bookmarks.vue';

library.add(faCheck, faTrashAlt);

export default {
    name: 'Bookmarks',

    components: { CoreBookmarks, Draggable },

    inject: ['i18n'],
};
</script>

<style lang="scss">
    .bookmarks {
        position: fixed;
        top: 52px;
        width: 100vw;
        display: flex;
        padding: 0.2em;
        -webkit-box-shadow: 0 1px 1px hsla(0,0%,4%,.2);
        box-shadow: 0 1px 1px hsla(0,0%,4%,.2);
        margin-left: 0;
        transition: margin-left .5s, width .5s;
        z-index: 2;

        &.with-menu {
            margin-left: 180px;
            width: calc(100vw - 180px);

            &.menu-collapsed {
                margin-left: 56px;
                width: calc(100vw - 56px);
            }
        }

        a.tag:hover {
            text-decoration: none;
        }

        .tag.check {
            margin-left: 0.1em;
        }

        .bookmark-items {
            position: relative;
            overflow-x: auto;
            overflow-y: hidden;

            .control:not(:last-child) {
                margin-right: .25em;
            }
        }
    }
</style>
