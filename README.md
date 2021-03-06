# Capybara

Capybara is a Ruby gem.

Capybara can mimic actions of real users interacting with web-based applications.

Capybara is commonly used inside the Cucumber framework.

It can be found here - https://rubygems.org/gems/capybara

To install 

![Picture of Capybara, Cucumber and Ruby](capybara.png)



## General Commands For Browsers

```
visit( 'http://www.google.co.uk')

sleep(2)
    
refresh()

if page.driver.browser.browser == :firefox
  page.driver.browser.manage.window.maximize()
elsif page.driver.browser.browser == :chrome
  page.driver.browser.manage.window.resize_to(1440, 800) # Chrome doesn't maximize() too good so used this instead
end

page.driver.browser.manage.window.maximize()

page.driver.browser.manage.window.resize_to( 1440, 800 )
    
page.execute_script "window.scrollBy(0,10000)" # Scroll down the page with the browser

within_window open_new_window do ### Opening New Tabs
  visit( 'http://google.co.uk' )
end

within_window open_new_window do
  visit( text )
end
```

 ## Verifying Content Exists On Page
 
```
find('#seek_in_halves').hover                       
    
find( "a" , text: "Opt out of the HTML5 Player" )   

find("h1").should have_content("Shoes")

# Expecting page to have certain text inside a certain div + tag
expect(page).to have_css("#generated_errors td", text: new_error_message )  
expect(page).to have_css("#generated_errors a" , text: new_playlist )
    
page.should have_content('lolacopterdaly')          # From ETSY, a field is populated from firstname + lastname fields
    
expect(page).to have_title "Cucumber - Wikipedia"   # Just part of the actual title "Cucumber - Wikipedia, the free encyclopedia"

assert_selector('div.heading', text: 'Create an Etsy account and start shopping') 

if page.first("#bbcprivacy-continue-button")
  click_button( "OK" )
end
    
if page.first(".p_subtitleButton")
  page.first(".p_subtitleButton").click
  sleep(1)
end
 ```


### Clicking SINGLE Elements

```
page.first('#derp').click                           # derp    is a button with an ID

page.first(".p_ctaIcon").click                      # ctaIcon is a button with a CLASS

click_link "Cucumber - Wikipedia"                   # First listed result from a Google search query

click_link( "Opt in to the HTML5 Player" )          # Another example of using click LINK
    
click_button( "OK" )

click_button( "Yes, I agree" )
  
find('a.pr-xs-3', :text => 'Shoes').click           # "Shoes" is text for an "a" element link under CLASS pr-xs-3

find("span", text: "Flash").hover

find( "a" , text: new_code ).click
```

### Clicking Multiple Elements

```
# Using table from Scenario Outline
When(/^I click on error code - "([^"]*)"$/) do |new_code|
  if new_code == "1005" or new_code == "3024"
    page.first( "a" , text: new_code ).click
  end
end

# Clicks on every single BUTTON in the class .refButton
page.all(:css, '.refbutton').each do |item|
  puts item.click
end
```

### Counting Number Elements On Page

```
page.all(".refbutton").count                        # Counts every instance of the class .refbutton

page.all("li").count                                # Counts all li elements on page

page.all(".pl-xs-0 li").count                       # Number of li elements INSIDE the class .pl-xs-0

page.all(".pl-xs-0 li").count.should eql(8)         # Different way instead of using IF statement                                   
```


### A11Y Checks

```
page.first(".p_pauseIcon").hover
expect(page.find('.p_pauseIcon')['aria-label']).to eq('Pause')

expect(page.find('button#p_subtitleSizeButton_useSmallestFontSize')['aria-pressed']).to eq("true")

within_frame 'smphtml5iframemp' do
    expect(page.find('#p_audioui_nextButton')['aria-label']).to eq('Next item')
    find_button('p_audioui_nextButton', disabled: true).should be
end
```  

### Verify CSS

```
within_frame 'smphtml5iframemp' do
    color = find('.p_cta').native.css_value('background-color')
    expect(color).to eq('rgba(0, 0, 0, 0.8)')
end

within_frame('wrapper') do
   color = find('body').native.css_value('background-color')
   expect(color).to eq('rgba(0, 1, 17, 1)')
end

cta_guidance = find('.p_guidanceText').native.css_value('font-family')
expect(cta_guidance).to eq('ReithSans, Arial, Helvetica, freesans, sans-serif')

x = find('h1').native.css_value('font-family')
expect(x).to eq("ReithSans, Arial, Helvetica, freesans, sans-serif")

cta_display = find('.p_iconHolder').native.css_value('display')
expect(cta_display).to eq('block')

cta_color = find('.p_iconHolder').native.css_value('background-color')
expect(cta_color).to eq('rgba(0, 0, 0, 0)') # Transparent

transition_time = find('.p_iconHolder').native.css_value('transition')
expect(transition_time).to eq('opacity 0.4s ease-out 0s')

hover_color = find('.p_controlBarButton.p_buttonHover').native.css_value('background-color')
expect(hover_color).to eq("rgba(187, 25, 25, 0.133)")
```

### Copy + Pasting

```
div_text = find('.recipe-share').text()        # To copy the word next to the input field

div_text = find('.recipe-share input').text()  # To copy what's in the field

find('#orb-search-q').set(div_text)            # This is buggy when pasting URLs
``` 
 

### Specifying actions within an iFrame

```
within_frame 'smpj2ooiframemedia_player' do      # This is an id
    page.first(".p_ctaIcon").click               # Click class
end
 ```

### Keyboard commands
  
```
find(:id, 'smpj2ooiframemedia_player').native.send_keys(:tab)

find(:id, 'smpj2ooiframemedia_player').native.send_keys(:arrow_down)

find(:id, 'smpj2ooiframemedia_player').native.send_keys(:arrow_up)

find(:id, 'smpj2ooiframemedia_player').native.send_keys(:enter)

find(:id, 'first-name').native.send_keys(:tab) # Example of hooking onto ID and tabbing from there!
```

###  Text-fields (Example from google.co.uk)

```
When( /^I am on Google$/ ) do
    fill_in 'q', :with => "cucumber\n" # q is the text-box, \n is equivalent to pressing ENTER
end

When(/^I enter all the fields with valid data$/) do
  page.first('#first-name').click
  fill_in 'first_name', :with => "LOLACOPTER"
end
```

### Executing Javascript

```
page.execute_script('embeddedMedia.players[0].play();')

page.execute_script( 'mediaPlayer.updateUiConfig({"controls": {"always": "true"}});')

page.execute_script( 'alert("Here is an alert!");')

page.driver.browser.switch_to.alert.accept

page.execute_script( 
                     'var volume_div = document.getElementById("button_menu");
                      volume_div.innerHTML += "<p> "+ mediaPlayer.volume() + "</p>";' 
                    )

final_volume = page.execute_script( 'return(mediaPlayer.volume() );')

if ( page.execute_script('return(mediaPlayer.muted());') == true )
  find(:id, 'smpj2ooiframemedia_player').native.send_keys(:enter)
  sleep(1)
end
```

### Chunk Of Code In env.rb File To Stop Killing Browser Session

```
Capybara::Selenium::Driver.class_eval do
  def quit
    puts "Press RETURN to quit the browser"
    $stdin.gets
    @browser.quit
  rescue Errno::ECONNREFUSED
    # Browser must have already gone
  end
end
```

### Ruby Asserts (Not Capybara Specific)

```
final_volume = page.execute_script( 'return(mediaPlayer.volume() );')

expect(final_volume).to eq(1)

expect(final_volume).to be_between(0.9, 1.1).inclusive

expect(final_volume).to be_between(0.9, 1.1)

expect(final_volume).to be <= 1
```    
   
    
    
    
