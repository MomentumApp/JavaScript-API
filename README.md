# JavaScript API

## Including in a page

Since jQuery is required for AJAX functionality, add jQuery and then the API script as follows:

    <!doctype html>
    <html>
        <head>
            <title>Evolution API Test Page</title>
            <script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.2/jquery.min.js"></script>
            <script src="/api-v1.js"></script>
        </head>
    </html>
    
Note: this assumes that you are on the same domain that Momentum is hosted on.

## Example Query

The following example will print the names of the first 12 Charities in a bullet list.

HTML code:

    <ul id="charities"></ul>
    <script>
        // Make a function to display charities
        function displayCharities(err, charities) {
            if(err)
                return alert('There was an error loading charities, ' + err);
            
            var list = '';
            charities.forEach( function(charity) {
                list += '<li>'+charity.name+'</li>';
            } );
            
            // Save the HTML into the list
            $('#charities').html(list);
        }
        
        // Load the charity list
        e.app.community.charities({page_length: 12}, displayCharities);
    </script>
    
## API Methods

Currently, only the community app is supported, but adding an app to the API couldn't be easier!

PHP Code:

    class App_Community extends App {
    
        public $_api_v1 = array(
            '@charity'   => 'charity',
            '@project'   => 'project',
            '@team'      => 'team',
            
            '@charities' => 'list_charities',
            '@projects'  => 'list_projects',
            '@teams'     => 'list_teams'
        );
    
The @ in front of the property means the property is loaded asynchronously and not when loading the API.
The values on the left are the javascript method names, i.e. `e.app.community.project`, and on the right
the property/method name in the current class to expose.

For an example where not all properties are aynchronous, take the example of community.charity.

PHP Code:

    class App_Community_Charity extends App_Community_Base {
    
        public $_api_v1 = array(
            'id' => 'id',
            'name' => 'name',
            'progress' => 'progress',
            'target' => 'target',
            'photo' => 'photo_url',
            '@projects' => 'projects'
        );

Here, basic information is exposed immediately when loading a charity, but the list of projects must be loaded afterwards.

### Loading a model

JavaScript Code to load project 10:
    e.app.community.project(10, function(err, project) {
        if(err)
            alert('Error, ' + err);
        else
            alert(project.name);
    });