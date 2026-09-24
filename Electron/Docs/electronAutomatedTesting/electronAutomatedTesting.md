### Electron Automated Testing

Electron 애플리케이션은 일반적인 웹 애플리케이션과 달리 Chromium 기반의 UI뿐만 아니라 Main Process, Renderer Process, Native API 등 여러 계층으로 구성되어 있음

따라서 단순히 함수 단위의 테스트만 수행하는 것보다 실제 애플리케이션을 실행한 상태에서 사용자의 동작과 애플리케이션의 결과를 검증하는 E2E 테스트가 중요함

→ 하지만 Electron 자체적으로 특정 테스트 도구를 제공하지 않음

<br/>

그렇기에 외부 테스트 도구를 사용하여 E2E 테스트를 구성해야함

대표적으로 다음과 같음 방법이 있음

- WebDriver
    - WebdriverIO
    - Selenium
- Platwright
- Custom Test Driver

<br/>
<br/>

### WebDriver - WebdriverIO & Selenium

WebDriver는 브라우저를 자동으로 제어하기 위한 표준 인터페이스임

웹 페이지를 열거나 사용자의 입력을 전달하고 JavaScript를 실행하는 등 실제 사용자의 브라우저 조작을 자동화할 수 있음

Electron 역시 Chromium을 기반으로 하기 때문에 WebDriver를 이용해 애플리케이션의 화면과 상호작용하는 테스트를 구성할 수 있음

<br/>

WebdriverIO는 Node.js 환경에서 WebDriver를 사용해 테스트를 작성할 수 있도록 만들어진 테스트 자동화 프레임워크임

테스트 러너뿐만 아니라 reporter, service 등 테스트 환경을 구성하는 다양한 기능을 제공함

처음 프로젝트에 추가할 때는 다음과 같이 설정할 수 있음

```bash
npm init wdio@latest .
```

Electron 애플리케이션을 테스트할 경우 `Desktop Testing - of Electron Applications` 을 선택하면 됨

<br/>

WebdriverIO를 Electron과 연결하기 위해 `wdio.conf.js` 에서 Electron 서비스를 활성화함

```tsx
export const config = {
	services: ['electron'],
	
	capabilities: [
		{
			browserName: 'electron',
			
			'wdio:electronServiceOptions': {
				appArgs: ['foo', 'bar=baz']
			}
		}
	]
}
```

`browserName: ‘electron’` 설정을 통해 WebdriverIO는 Chrome 브라우저가 아니라 Electron 애플리케이션을 테스트 대상으로 사용함

<br/>

WebdirverIO에서는 실제 화면의 요소를 가져와 사용자의 동작을 수행할 수 있음

다음은 키보드 입력을 테스트할 경우의 예시 코드임

```tsx

import { browser, $, expect } from '@wdio/globals'

describe('keyboard input', () => {
  it('should detect keyboard input', async () => {
    await browser.keys(['y', 'o'])
    
    await expect($('keypress-count')).toHaveText('YO')
  })
})
```

테스트 코드가 실제 애플리케이션에 키보드 입력을 전달하고, 화면의 `keypress-count` 요소에 `YO` 라는 텍스트가 표시되는지를 검증함

즉 사용자 입력 → 애플리케이션 동작 → 화면의 결과라는 실제 사용 흐름을 테스트할 수 있음

<br/>

WebdriverIO의 또 다른 특징은 Electron API에도 접근할 수 있다는 점임

다음과 같이 테스트 코드에서 현재 포커스된 `BrowserWindow` 를 가져온 뒤 Electron의 `dialog` API를 직접 호출할 수도 있음

```tsx
await browser.electron.execute(
  (electron) => {
    const appWindow = electron.BrowserWindow.getFocuseWindow()
    
    electron.dialog.showMessageBox(appWindow, {
      message: 'Hello World!'
    })
  }
)
```

즉 WebdriverIO는 단순히 Renderer의 DOM만 테스트하는 것이 아니라, 필요한 경우 Electron의 동작까지 테스트 코드에서 검증할 수 있음

<br/>

작성한 테스트는 다음 명령어로 실행할 수 있음

```bash
npx wdio run wdio.conf.js
```

WebdriverIO는 테스트를 실행하면서 Electron 애플리케이션을 실행하고 테스트가 끝난 후 애플리케이션을 종료하는 과정도 처리함

→ 테스트 코드에서 실행, 종료 로직을 작성할 필요가 없음

<br/>

Selenium은 여러 언어에서 WebDriver API를 사용할 수 있도록 제공하는 웹 자동화 프레임워크이며, Node.js에서는 `selenium-webdriver` 패키지를 사용할 수 있음

Electron에서 Selenium을 사용하려면 먼저 Electron에 맞는 ChromeDriver를 실행해야 함

```bash
npm install --save-dev electron-chromedriver
```

그다음 ChromeDriver 서버를 실행함

→ 기본적으로 9515 포트에서 실행됨

<br/>

그 다음 `selenium-webdriver` 를 설치함

```bash
npm install --save-dev selenium-webdriver
```

<br/>

그리고 테스트 코드에서 ChromeDriver의 주소와 Electron 실행 파일의 경로를 직접 지정함

```tsx
const webdriver = require('selenium-webdriver)

const driver = new webdriver.Builder()
	.usingServer('http://localhostL9515')
	.withCapabilities({
		'goog:chromeOptions': {
			binary: '/Path-to-Youre-App.app/Contents/MacOS/Electron'
		}
	})
	.forBrowser('chrome')
	.build()
```

앞서 살펴본 WebdriverIO와 비교하면 Selenium에서는 ChromeDriver 서버를 직접 실행하고 Electron 바이너리 경로도 직접 지정해야 한다는 차이가 있음

<br/>
<br/>

### PlayWright

Platwright는 Chromium과 같은 브라우저의 원격 디버깅 프로토콜을 사용해 브라우저를 제어하는 E2E 테스트 프레임워크임

Electron에서는 Chrome DevTools Protocol을 이용해 Electron 애플리케이션을 제어함

다음 명령어를 입력해 Playwright를 설치할 수 있음

```bash
npm install --save-dev @playwright/test
```

Playwright는 테스트 실행기도 함께 제공하기 때문에 별도의 테스트 러너를 추가로 구성할 필요가 없음

<br/>

Playwright에서는 `_electron.launch()` 를 이용해 Electron 애플리케이션을 실행할 수 있음

```bash
import { test, _electron as electron } from '@platwright/test'

test('launch app', async () => {
	const electron App == await electron.launch({
		args: ['.']
	})
	
	await electronApp.close()
})
```

여기서 `electron.launch()` 가 Electron 애플리케이션을 실행하고 `ElectronApp` 객체를 반환함

즉 테스트 코드가 직접 Electron 프로세스를 실행하는 것이 아니라 Playwright가 테스트 대상 Electron 애플리케이션을 실행해줌

<br/>

Playwright에서 반환되는 `ElectronApp` 객체를 사용하면 다음과 같이 Main Process의 정보에도 접근할 수 있음

```tsx
const isPackaged = await electronApp.evaluate(async ({ app }) => {
	return app.isPackaged
})
```

`evaluate()` 는 Electron 애플리케이션의 Main Process에서 실행되지만 Playwright를 통해 Main Process의 Electron API도 검증할 수 있음

<br/>

Playwright는 Electron의 `BrowserWindow` 와 연결된 `Page` 객체를 생성할 수도 있음

```tsx
const window = await electronApp.firstWindow()
```

<br/>

이후에는 일반적인 Playwright 테스트처럼 화면을 제어할 수 있음

```tsx
await window.screenshot({
	path: 'intro.png'
})
```

즉 Electron의 `BrowserWindow` 를 Playwright가 제어할 수 있는 `Page` 객체로 연결되는 것임

<br/>

앞의 내용을 하나로 합치면 다음과 같은 형태가 됨

```tsx
import { test, expect, _electron as electron } from '@playwright/test'

test('example test', async () => {
  const electronApp = await electron.launch({
    args: ['.']
  })
  
  const isPackaged = await electronApp.evaluate(async ({ app }) => {
    return app.isPackaged
  })
  
  expect(isPackaged).toBe(false)
  
  const window = await electronApp.firstWindow()
  
  await window.screenshot({
    path: 'intro.png'
  })
  
  await electronApp.close()
})
```

<br/>

테스트는 다음 명령어로 실행할 수 있음

```bash
npx playwright test
```

Playwright Test는 기본적으로 `*.test.js` , `*.spec.js` , `*.test.ts` , `*.spec.ts` 등의 테스트 파일을 찾아 실행하며 TypeScript도 별도의 추가 설정 없이 사용할 수 있음

<br/>
<br/>

### Custom Test Driver

Electron에서는 WebDriver나 Playwright 같은 외부 테스트 프레임워크를 사용하는 방법 외에도 직접 테스트 드라이버를 만들 수 있음

Node.js의 `child_process` 와 IPC-over-STDIO를 이용해 테스트 코드와 Electron 애플리케이션 사이에 직접 통신 구조를 만드는 방식임

먼저 테스트 코드에서 Electron 프로세스를 자식 프로세스로 실행함

```bash
const childProcess = require('node:child_process')

const appProcess = childProcess.spawn(
	electronPath,
	['./app'],
	{
		stdio: [
			'inherit',
			'inherit',
			'inherit',
			'ipc'
		]
	}
)
```

여기서 중요한 부분은 `stdio` 와 `ipc` 임

이를 통해 다음과 같이 테스트 프로세스와 Electron 프로세스 사이에서 메시지를 주고받을 수 있음

<br/>

테스트 코드에서 `process.send(’message’)` 를 이용해 메시지를 보낼 수 있음

```tsx
process.send({
	my: 'message'
})
```

<br/>

반대로 `process.on(’message’)` 를 이용해 해당 메시지를 받을 수 있음

```tsx
process.on('message', (msg) => {
	// ...
})
```

즉 Electron 애플리케이션과 테스트 코드 사이에 직접 메시지를 주고받는 것임

<br/>