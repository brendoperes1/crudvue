<template>
    <div class="tag-input">
        <div v-for="(tag, index) in tags"
            :key="tag"
            class="tag-input__tag">
            <span @click="removeTag(index)">x</span>
            {{ tag }}
        </div>
        <!-- podia ser colocado uma mascara aqui também, para caso do ddd, ou feito algo separado na esquerda -->
        <input 
            type="text"
            maxlength="9"
            placeholder="Digite o telefone"
            class="tag-input__text"
            @keydown.enter="addTag"
            @keydown.delete="removeLastTag"
            @blur="send"/>
    </div>
</template>

<script>
export default {
    props: {
        hasPhone: {
            type: Text,
        }
    },
    data() {
        return {
            tags: []
        }
    },
    methods: {
        send(event){
            event.preventDefault();
            var val = event.target.value.trim();
            if (val.length > 0) {
                this.tags.push(val);
                event.target.value = '';
            }
            this.$emit('phoneTag', { 
              phone: this.tags
            });
        },
        addTag (event) {
            event.preventDefault();
            var val = event.target.value.trim();
            if (val.length > 0) {
                this.tags.push(val);
                event.target.value = ''
            }
        },
        removeTag (index) {
            this.tags.splice(index, 1);
        },
        removeLastTag(event) {
            if (event.target.value.length === 0) {
                this.removeTag(this.tags.length - 1);
            }
        }
    }
}
</script>

<style scoped>
.tag-input {
    width: 100%;
    /* border: 1px solid #eee; */
    font-size: 0.9em;
    height: 50px;
    box-sizing: border-box;
    /* padding: 0 10px; */
    margin-bottom: 20px;
    padding: 12px 20px;
    border: 2px solid;
    border-radius: 8px;
}

.tag-input__tag {
    height: 30px;
    float: left;
    margin-right: 10px;
    background-color: #eee;
    margin-top: -4px;
    line-height: 30px;
    padding: 0 5px;
    border-radius: 5px;
}

.tag-input__tag > span {
    cursor: pointer;
    opacity: 0.75;
}

.tag-input__text {
    border: none;
    outline: none;
    font-size: 0.9em;
    /* line-height: 50px; */
    background: none;
    /* margin-bottom: 20px; */
    /* margin-bottom: 20px;
    padding: 12px 20px;
    border: 2px solid;
    border-radius: 8px; */
}
</style>