<template>
    <tr>
        <td class="d-flex al-left d-direction-column">

            <div>
                <input @change="handler($event)" :name="`status_${uid}`" :checked="status == 0 ? 'checked' : ''" :id="`positive_${uid}`" type="radio" />
                <label :for="`positive_${uid}`">Sim</label>
            </div>

            <div>
                <input @change="handler($event)" :name="`status_${uid}`" :checked="status == 1 ? 'checked' : ''" :id="`negative_${uid}`" type="radio" />
                <label :for="`negative_${uid}`">Não</label>
            </div>

            <div>
                <input @change="handler($event)" :name="`status_${uid}`" :checked="status == 2 ? 'checked' : ''" :id="`nosense_${uid}`" type="radio" />
                <label :for="`nosense_${uid}`">Não se aplica</label>
            </div>

        </td>
        <td>
            {{question}}
        </td>
    </tr>
</template>

<script>
export default {
    name: 'QuestionRow',
    props: {
        question: String,
        status: Number,
        uid: Number
    },
    methods: {
        handler(event) {
            const options = {
                positive: 0,
                negative: 1,
                nosense: 2
            }
            const current = event.target.id.split('_')[0]
            const status = options[current]
            const response = {
                status: status,
                uid: this.uid
            }
            this.$emit('handlestatus', response)
        }
    }
}
</script>