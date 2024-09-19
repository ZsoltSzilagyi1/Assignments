<template>
    <tr>
        <td class="listID">#{{ id }}</td>
        <td class="listName">{{ name }}</td>
        <td class="listNumber">{{ phone }}</td>
        <td class="listEdit"><button type="button" class="btn btn-info">Edit</button></td>
        <td class="listDelete"><button type="button" class="btn btn-danger">Delete</button></td>
    </tr>
</template>
<script>
import TopBar from './TopBar.vue';

export default {
    name: 'TableRow',
    props:['id','name','phone','test'],
    
    
}

</script>