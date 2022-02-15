# LFRGS - Dynamic List Renderer
Render and filter entries using a mustache template


## Intro
Displaying some sort of list of entries considering filters that may be defined by the user. 
These user-defined filters will ofter change how the request is made, and DLR was creating 
visioning to assist into this issue  requesting and rendering entries. The request can be altered using filters

## Basic usage
```html

    <div id="team-members-list-renderer"> <!-- A main wrapper with some id -->
            
        <form data-dynamic-entries-renderer-form> <!-- All filter fields should be inside a form element with this data attribute -->

            <!-- Each filter field should declare its parameter key and default value -->
            <input 
                type="text" 
                data-dynamic-entries-renderer-filter-parameter="param1" 
                data-dynamic-entries-renderer-filter-default-value=""
            />
            <input 
                type="text" 
                data-dynamic-entries-renderer-filter-parameter="param2" 
                data-dynamic-entries-renderer-filter-default-value="test"
            />

        </form>

        <!-- A results area: this is where the entries will be rendered -->
        <div data-dynamic-entries-renderer-results></div>

    </div>

    <script>


            var my_dynamic_entries_renderer = new DynamicEntriesRenderer('my_dynamic_entries_renderer', {

                requestFn: function(requestParams, render) {

                    let pageN = Number(requestParams.p);
                    let delta = Number(requestParams.delta);
            
                    var myFetch = fetch('http://yourservice.com?p1='+requestParams.param1+'&p2='+requesr, { 
                        method: 'GET',
                        headers: {
                            'app-id': '61cc1e2f679cb9641fa3f88f'
                        }
                    })
                    .then(response => {
                        return response.json()
                    })
                    .then(response => {
                        render({
                            entries: response.data,
                            hasEntries: (response.data.length > 0),
                            total: response.total,
                            start: (pageN * delta) - (delta - 1),
                            end: (pageN * delta) - (delta - response.data.length),
                        })

                    }).catch(error => {
                        render({
                            error: true
                        })

                    })
                
                }, 

                onRequestStart: function(requestParams) {

                    // This is triggered right before a request.
                    // You may use it to show some loading feedback

                },

                onRender: function() {
                    // This is triggered right after a render occurs.
                    // Here you can hide your loading effects and do stuff to what's been rendered 
                },
        

                template: document.getElementById('template').innerHTML,
                templateType: "mustache"


            });
    </script>

```

### Inicializar a Demo
```shell
npm run start
```