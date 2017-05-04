from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
import datetime
import time
import os

today = datetime.date.today()
first = today.replace(day=1)
lastMonth = first - datetime.timedelta(days=1)
firstDay = lastMonth.replace(day = 1)
startDate = firstDay.strftime('%m/%d/%Y %H:%M')
endDate = first.strftime('%m/%d/%Y %H:%M')
dateName = firstDay.strftime('%B')

currentUser = os.popen("whoami").read()
endofUser = len(currentUser)
endofUser = endofUser - 1
currentUser = currentUser[:endofUser]

os.system("mkdir /Users/" + currentUser + "/" + dateName)

Servers = {'servername':'drivename'}

for i, value in Servers.iteritems():
#Set up
	profile = webdriver.FirefoxProfile()
	profile.set_preference('print.always_print_silent', True)
	driver = webdriver.Firefox(profile)
	driver.get("https://nagios.url.net/pnp4nagios/index.php/graph?host=" + i + "&srv=_HOST_")
	driver.maximize_window()

	linuxCPU = "a[title='linux-cpu']"
	basketCPU = "a#" + i + "\:\:linux-cpu\:\:0"
	linuxMemory = "a[title='linux-swap']"
	basketMemory = "a#" + i + "\:\:linux-swap\:\:0"
	linuxDisk = "a[title='linux-disk']"
	basketDisk = "a#" + i + "\:\:linux-disk\:\:0"
	basketAddDisk = "//tr[td='Datasource: /" + value + "']/td/span[@id=\"basket_action_add\"]"
	linuxUptime = "a[title='linux-uptime']"
	basketUptime = "a#" + i + "\:\:linux-uptime\:\:0"
	basketSubmit = "button#basket-show"
	customDate = "a[title='Define a custom time range']"
	dateStart = "input#dpstart"
	dateEnd = "input#dpend"
	submitDate = "input#submit"
	viewPDF = "a[title='View PDF']"
	downloadPDF = "button#print"

#Collect basket items

	linuxCPUID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(linuxCPU))
	linuxCPUID.click()

	basketCPUID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(basketCPU))
	basketCPUID.click()

	linuxMemoryID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(linuxMemory))
	linuxMemoryID.click()

	basketMemoryID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(basketMemory))
	basketMemoryID.click()

	linuxDiskID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(linuxDisk))
	linuxDiskID.click()

	basketDiskID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(basketDisk))
	basketDiskID.click()

	basketAddDiskID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_xpath(basketAddDisk))
	basketAddDiskID.click()

	linuxUptimeID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(linuxUptime))
	linuxUptimeID.click()

	basketUptimeID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(basketUptime))
	basketUptimeID.click()

	time.sleep(1)

#Checkout
	basketSubmitID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(basketSubmit))
	basketSubmitID.click()

	customDateID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(customDate))
	customDateID.click()

	time.sleep(1)

	dateStartID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(dateStart))
	dateStartID.send_keys(startDate)

	dateEndID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(dateEnd))
	dateEndID.send_keys(endDate)

	submitDateID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(submitDate))
	submitDateID.click()

	viewPDFID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(viewPDF))
	viewPDFID.click()

	time.sleep(1)

	downloadPDFID = WebDriverWait(driver, 60).until(lambda driver: driver.find_element_by_css_selector(downloadPDF))
	downloadPDFID.click()

	time.sleep(5)


	driver.quit()


	outputName = os.popen("ls /private/var/spool/pdfwriter/" + currentUser + "/").read()
	outputEnd = (outputName.find('.pdf'))
	outputEnd = outputEnd + 4
	outputName = outputName[:outputEnd]
	os.rename("/private/var/spool/pdfwriter/" + currentUser + "/" + outputName, "/Users/" + currentUser + "/" + dateName + "/" + i + ".pdf")

