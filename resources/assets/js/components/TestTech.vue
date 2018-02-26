<template>
    <div class="container">
        <br>
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">TestTech</div>

                    <div class="card-body">
                        <div class="form-group">
                            <textarea name="content" v-model="content" class="form-control"></textarea>
                        </div>
                        <div>
                            <strong>Raw</strong> <br>
                            {{ content }} <hr>
                            <strong>Generated</strong> <br>
                            {{ cleanText }}
                        </div>
                    </div>
                </div>
                <hr>
                <ul class="list-group">
                    <li v-for="word in vars" :key="word" class="list-group-item">
                        <div class="form-group row">
                            <label class="col-sm-2">{{ word }}</label>
                            <div class="col-sm-8">
                                <input type="text" class="form-control" v-on:input="generateValues(word, $event.target.value)">
                            </div>
                        </div>
                    </li>
                </ul>
                <hr>
            </div>
        </div>
    </div>
</template>

<script>
// ref: https://coderwall.com/p/nilaba/simple-pure-javascript-array-unique-method-with-5-lines-of-code
Array.prototype.unique = function() {
  return this.filter(function (value, index, self) { 
    return self.indexOf(value) === index;
  });
}

function findVariables(text) {
    const words = text.split(' ');
    const vars = words.filter(word => word.charAt(0) === '#');
    return vars.unique();
}

function findAndReplaceWords(text, obj) {
    const re = new RegExp(Object.keys(obj).join('|'), 'gi');

    // ref: https://stackoverflow.com/a/15604206
    const cleaned = text.replace(re, function(matched) {
        return obj[matched];
    });

    return cleaned;
}

export default {
    data: function() {
        return {
            content: 'Halo my name is Hiraq',
            cleanVars: {},
            cleanText: '',
        }
    },
    methods: {
        generateValues: function(name, value) {
            this.cleanVars[name] = value;
            this.generateCleanText();
        },
        generateCleanText: function() {
            this.cleanText = findAndReplaceWords(this.content, this.cleanVars);
        }
    },
    computed: {
        vars: function() {
            return findVariables(this.content);
        },
    }
}
</script>
