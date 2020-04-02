---
description: Scraping the web and notifying to Slack
---

# Web Scraping with Python

### crontabの設定

{% code title="crontab -l" %}
```text
PATH=/usr/local/bin

* * * * * cd ~/Developer/python/scraping/ ; python3 test.py 2>> error.txt
```
{% endcode %}

```text
$ crontab -l # List cron jobs
$ crontab -e # Edit cron jobs
$ crontab -r # Remove cron jobs
```

### Python code

```python
from bs4 import BeautifulSoup
import urllib.request
from config import config, update_recent_text
from slack_notification import slack_notify

url = config['web_info']['url']
html = urllib.request.urlopen(url).read()

soup = BeautifulSoup(html, "lxml")

# print(html)

def extract_paragraphs(soup=soup):
	paragraphs = soup.find_all("p")
	return paragraphs

def extract_uls(soup=soup):
	uls = soup.select("main ul")
	return uls

def compare_texts(new_text):
	old_text = config['web_info']['recent_text']
	print(type(old_text))
	if new_text == old_text:
		print('same')
	else:
		update_recent_text(new_text)
		print('updated')
		slack_notify(text_=new_text, link_=url)

if __name__ == "__main__":
	print(soup.title.string)
	# print(config['web_info']['recent_text'])
	# slack_notify(text_=paragraphs[1].get_text())
	# for i, p in enumerate(extract_uls()):
	#	print(str(i) + ": " + p.get_text())
	new_text = str(extract_uls()[0]).replace('\n', '')
	print(new_text)
	compare_texts(new_text)
```

### Slack Notification

{% code title="slack\_notification.py" %}
```python
import slackweb
from config import config

slack = slackweb.Slack(url=config['slack_info']['in_webhook_url'])

def slack_notify(text_="", link_=""):
	attachments = []
	attachment = {"title": link_, "pretext": "Website updated!", "text": "```" + text_ + "```", "mrkdwn_in": ["text", "pretext"], "color": "good"}
	attachments.append(attachment)
	slack.notify(attachments=attachments)

def slack_notify_text(text_="", link_=""):
	slack.notify(text=link_ + text_)
```
{% endcode %}

### Config file and config.py

{% code title="config.py" %}
```python
import configparser
import re

config = configparser.ConfigParser()
config.read('config.ini')

def update_recent_text(new_text):
	with open('config.ini', 'r') as f:
		lines = f.readlines()
	with open('config.ini', 'w') as f:
		for line in lines:
			if re.match(r'(recent_text = )', line):
				f.write("recent_text = {}".format(new_text))
				continue
			f.write(line)

```
{% endcode %}

{% code title="config.ini" %}
```python
[slack_info]
in_webhook_url = https://hooks.slack.com/services/****/*****

[web_info]
test_url = http://****/
url = https://****//

recent_text = test
```
{% endcode %}

