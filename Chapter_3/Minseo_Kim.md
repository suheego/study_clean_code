# ๐ก ์  3์ฅ ํจ์

## 1. ์๊ฒ ๋ง๋ค์ด๋ผ

### 1-1. ํ ๊ฐ์ง ๊ธฐ๋ฅ๋ง ํด๋ผ

- ํจ์๋ ๋๋๋ก ์ ์ ์์ ์ฝ๋๋ก ํ ๊ฐ์ง ๊ธฐ๋ฅ๋ง์ ๊ฐ์ง ํจ์๊ฐ ์ข๋ค. ๊ทธ๋ ๋ค๋ฉด ์ผ๋ง๋ ์งง์์ผ ํ ๊น? 2-5์ค ์ ๋๊ฐ ์ฌ์ค ๋งค์ฐ ์ข๋ค. ํจ์๊ฐ ๊ธธ๋ฉด ๊ธธ์๋ก ์ด๋ ํ ๊ฐ์ง ๊ธฐ๋ฅ ์ด์์ ๊ฒ์ ํ๊ณ  ์๋ค๋ ๋ฐ์ฆ์ด๊ธฐ๋ ํ ์์ด๋๊น.

### 1-1. ๋ถ์ ํจ๊ณผ๋ฅผ ์ผ์ผํค์ง ๋ง๋ผ

- ์๋ checkPassword ํจ์๋ ๊ฒ๋ณด๊ธฐ์ ํจ์ค์๋๊ฐ ๋ง๋ ์ง ํ์ธํ๋ ํจ์์ธ ๊ฒ์ฒ๋ผ ๋ณด์ด์ง๋ง ์ฌ์ค ์ธ์ ์ ๋ณด๋ฅผ ์ด๊ธฐํ ํ๋ ๋ถ์์ ์ธ ํจ๊ณผ๋ฅผ ์ผ์ผํจ๋ค. ์ด๋ฆ๋ง ๋ด์๋ ์ธ์์ ์ด๊ธฐํ ํ๋ค๋ ์ฌ์ค์ด ๋ค์ด๋์ง ์๋๋ค. ๋ํ ๊ทธ ์ฌ์ค์ ์จ๊ธฐ๊ณ  ์๋ ๊ฒ์ด ๋ ํฐ ์ํ์ ์ด๋ํ๋ค.

```java
public class UserValidator {
	private Cryptograher cryptographer;

	public boolean checkPassword (String userName, String password) {
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codePhrase = user.getPhraseEncodedByPassword();
			String phrase = cryptographer.decrypt(codePhrase, password);
			if ("Valid Password".equals(phrase)) {
				Session.initialize(); /// ์ธ์ ์ ๋ณด ์ด๊ธฐํ !?
				return true;
			}
		}
		return false;
	}

}
```

## 2. ํจ์ ๋น ์ถ์ํ ์์ค์ ํ๋๋ก

- ํจ์๊ฐ ํ์คํ ํ ๊ฐ์ง ๊ธฐ๋ฅ์ ํ๊ธฐ ์ํด์๋ ํด๋น ํจ์ ๋ชจ๋  ๋ฌธ์ฅ์ ์ถ์ํ ์์ค์ ํ๋๋ก ๋์ผํด์ผํ๋ค. ํ ํจ์ ๋ด์ ์ถ์ํ ์์ค์ ์์ผ๋ฉด ์ฝ๋๋ฅผ ์ฝ๋ ์ฌ๋์ด ํท๊ฐ๋ฆฐ๋ค. ํน์  ํํ์ด ๊ทผ๋ณธ ๊ฐ๋์ธ์ง ์๋๋ฉด ์ธ๋ถ์ฌํญ์ธ์ง ๋ค์๊ธฐ ์์ํ๋ฉด, ๊นจ์ด์ง ์ฐฝ๋ฌธ์ฒ๋ผ ์ฌ๋๋ค์ด ํจ์์ ์ธ๋ถ์ฌํญ์ ์ ์  ๋ ์ถ๊ฐํ๋ค.

### 2-1. ์์์ ์๋๋ก ์ฝ๋ ์ฝ๊ธฐ: ๋ด๋ ค๊ฐ๊ธฐ ๊ท์น

์ฝ๋๋ ์์์ ์๋๋ก ์ด์ผ๊ธฐ์ฒ๋ผ ์ฝํ์ผ ์ข๋ค. ํ ํจ์ ๋ค์์๋ ์ถ์ํ ์์ค์ด ํ ๋จ๊ณ ๋ฎ์ ํจ์๊ฐ ์จ๋ค. ์ฆ, ์์์ ์๋๋ก ํ๋ก๊ทธ๋จ์ ์ฝ์ผ๋ฉด ํจ์ ์ถ์ํ ์์ค์ด ํ ๋ฒ์ ํ ๋จ๊ณ์ฉ ๋ฎ์์ง๋ค. ์ฝ๋๋ฅผ ์ฝ๋ ๊ฒ์ด ํ๋์ ๋ฌธ๋จ์ ์ฝ๊ณ  ๋ค์ ๋ฌธ๋จ์ ์์ฐ์ค๋  ๋ด๋ ค๊ฐ ์ดํด๋๋ฏ ์์ฐ์ค๋ฝ๊ฒ ์ฝํ์ผ ํ๋ค. ์ฆ, ํจ์์ ๋ชจ๋  ๋ฌธ์ฅ์ด ์ถ์ํ ์์ค์ด ๋ณต์กํ๊ฒ ์ฝํ์ง ์์ ์์ด์ผ ํ๋ค.

## 3. ํจ์ ์ธ์๋ ์ ์ ์๋ก ์ข๋ค

- X, Y ์ขํ์ ๊ฐ์ ๋ ๊ฐ์ ์ธ์๊ฐ ํ๋์ ๊ฐ๋์ ํํํ๋ ๊ฒฝ์ฐ๊ฐ ์๋๋ผ๋ฉด ์ต๋ํ ํจ์ ์ธ์๋ 3๊ฐ๋ณด๋ค 2๊ฐ๊ฐ 2๊ฐ๋ณด๋ค 1๊ฐ, 1๊ฐ๋ณด๋ค 0๊ฐ๊ฐ ์ข๋ค. ์ธ์๊ฐ ๋ง์ ์๋ก ํจ์ ๋ก์ง์ ๊ฒฝ์ฐ์ ์๊ฐ ๋ง์์ ธ ํ์คํธ ์ฝ๋๋ฅผ ์งค ๋์๋ ๋ชจ๋  ๊ฒฝ์ฐ๋ฅผ ๊ณ ๋ คํ์ฌ ์ง์ผ ํ๊ธฐ ๋๋ฌธ์ ์ ์ฒด์ ์ผ๋ก ๋ณต์ก์ฑ๊ณผ ํจ์์ ๋ํ ๋ณผ๋ฅจ์ด ์ปค์ ธ side effect ๊ฐ ์ปค์ง๋ค.

## 4. ์์ ์ ์ธ ์ด๋ฆ์ ์ฌ์ฉํ๋ผ

- ํจ์๊ฐ ์๊ณ  ๋จ์ํ ์๋ก ์์ ์ ์ธ ์ด๋ฆ์ ์ง๊ธฐ๋ ์ฌ์์ง๋ค. ์งง๊ณ  ์ดํดํ๊ธฐ ์ด๋ ค์ด ์ด๋ฆ์ผ์๋ก ์ค์  ํจ์ ์ฝ๋๋ฅผ ์ฝ๊ธฐ ์ ๋ถํฐ ๋ต๋ตํด์ง๊ฑฐ๋ ๋งค์ฐ ๊ธฐ๋๊ธด ํจ์๋ฅผ ์ฝ๊ณ ๋ ํ์ฌ ์ํฉ์ ํด๊ฒฐํ  ์ ์๋ ์ํฉ์ ์ธ์งํ๊ฒ ๋๋ ์๊ฐ ์จ์ด ํฑ ๋งํ๋ ๊ทธ๋ฐ ๊ฒฝํ ํ ๋ฒ์ฏค ๊ฐ๋ฐ์๋ผ๋ฉด ์์๊น. ํ์์ ์์ด์๋ ํจ์์ ์ดํด๋๋ฅผ ๋์ผ ์ ์๋ ์์ ์ ์ธ ํจ์๋ช์ด ํจ์ฌ ํจ์จ์ ์ด๋ค.

## 5. ๋ช๋ น๊ณผ ์กฐํ๋ฅผ ๋ถ๋ฆฌํ๋ผ

- ํจ์๋ ๋ญ๊ฐ๋ฅผ ์ํํ๊ฑฐ๋ ๋ญ๊ฐ์ ๋ตํ๊ฑฐ๋ ๋ ์ค ํ๋๋ง ํด์ผํ๋ค. ๋ ๋ค ํ๋ ๊ฒฝ์ฐ ํผ๋์ ์ด๋ํ๋ค.

```java
public boolean set (String attribute, String value);
if (set("username", "unclebob")) ...
if (setAndCheckIfExits("username", "unclebob"))..
```

- ์์ set ํจ์๋ attribute ์์ฑ์ ์ฐพ์ ๊ฐ์ value๋ก ์ค์ ํ ํ, ์ฑ๊ณตํ๋ฉด true ํน์ ์คํจํ๋ฉด false ๋ฅผ ๋ฐํํ๋ค. ์ค์  ํจ์๋ฅผ ํธ์ถํ๋ ์ฝ๋๋ฅผ ์ฝ์ด๋ณด์๋ฉด username ์ unclebob ์ผ๋ก ์ค์ ํ๋ค๋ ๊ฒ์ธ์ง ํน์ ์ค์  ๋์ด์๋์ง ํ์ธํ๋ ๊ฒ์ธ์ง ์๊ธฐ ์ด๋ ต๋ค. ๊ทธ๋ ๊ธฐ์ setAndCheckIfExists ๋ผ๊ณ  ํจ์๋ช์ ๋ณ๊ฒฝํ์ฌ ํจ์์ ๊ธฐ๋ฅ์ ๋ชํํ ํ๋ ๊ฒ๋ ์ข์ง๋ง if ๋ฌธ์ ๋ฃ์ด ๋ด๋ ๊ทธ๋ฆฌ ์์ฐ์ค๋ฝ์ง ๋ชปํ๋ค.

```java
if (attributExists("username")) {
	setAttribute("username", "unclebob");
}
```

- ์์ ํจ์๋ username์ด ์๋์ง ํ์ธํ๋ ์กฐํ ๊ธฐ๋ฅ์ attributExists ์ ๋ถ์ฌํด ํ์ธํ ๋ค, setAttribute ํจ์๋ฅผ ํตํด ํด๋น unclebob ์ด๋ผ๋ ๊ฐ์ username์ ์ค์ ํ  ์ ์๋๋ก ํ๋ค. ์ด๋ ๊ฒ ์กฐํ์ ๋ช๋ น์ ๋ถ๋ฆฌํ๋ ๊ฒ์ด ํจ์๋ฅผ ์ฝ์ด ๋ด๋ ค๊ฐ๋ฉฐ ์ดํดํ๊ธฐ์ ํผ๋์ ์ผ๊ธฐํ์ง ์๋๋ค.

## 6. ์ค๋ฅ ์ฝ๋๋ณด๋ค ์์ธ๋ฅผ ์ฌ์ฉํ๋ผ ( try...catch )

- ํจ์์์ ์ค๋ฅ ์ฝ๋๋ฅผ ๋ฐํํ๋ ๋ฐฉ์์ ๋ช๋ น/์กฐํ ๋ถ๋ฆฌ ๊ท์น์ ์๋ฐํ๋ค.

```java
if (deletePage(page) == E_OK) {
	if (registry.deleteReference(page.name) == E_OK) {
		if (configKeys.deleteKey(page.name.makeKey())) == E_OK) {
...
```

- ์ ์ฝ๋๋ ๋์ฌ / ํ์ฉ์ฌ ํผ๋์ ์ผ์ผํค์ง ์๋ ๋์  ์ฌ๋ฌ ๋จ๊ณ๋ก ์ค์ฒฉ๋๋ ์ฝ๋๋ฅผ ์ผ๊ธฐํ๋ค. ์ค๋ฅ ์ฝ๋๋ฅผ ๋ฐํํ๋ฉด ํธ์ถ์๋ ์ค๋ฅ ์ฝ๋๋ฅผ ๊ณง ๋ฐ๋ ์ฒ๋ฆฌํด์ผ ํ๋ค๋ ๋ฌธ์ ์ ๋ถ๋ชํ๋ค.

```java
try {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKery(page.name.makeKey());
} catch (Exception e) {
	logger.log(e.getMessage());
}
```

- try/catch ํจ์๋ก ์ค๋ฅ ์ฒ๋ฆฌ ์ฝ๋๊ฐ ์๋ ์ฝ๋์์ ๋ถ๋ฆฌ๋์ด ์ฝ๋๊ฐ ๊น๋ํด์ง๋ค. ๊ทธ๋ฌ๋ ์ฝ๋ ๊ตฌ์กฐ์ ํผ๋์ ์ผ์ผํค๋ฉด ์ ์ ๋์๊ณผ ์ค๋ฅ ์ฒ๋ฆฌ ๋์์ ๋ค์๋๋ค.

### try/ catch ๋ธ๋ก์ ๋ณ๋ ํจ์๋ก ๋ถ๋ฆฌํ๋ผ

```java
public void delete (Page page) {
	try {
		deletePageAndAllReferences(page);
	} catch (Exception e) {
		logError(e);
	}
}

private void deletePageAndAllReferences (Page page) throws Exception {
	deletePage(page);
	registry.deletePageAndAllReferences(page.name);
	configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
	logger.log(e.getMessage());
}
```

- try/catch ๋ก ๋ถ๋ฆฌ ๊ฐ๋ฅํ ์ญ์  ๊ธฐ๋ฅ์ ๊ฐ์ง delete ํจ์๋ฅผ ์๋ก ๋ง๋ค๊ณ  deletePageAndAllReferences ๋ผ๋ ํจ์๋ฅผ ํตํด ์ค์  ํ์ด์ง๋ฅผ ์ญ์ ํ๋ ๊ธฐ๋ฅ์ ์ํํ๊ณ , ์๋ฌ๋ฅผ ๋ฐํํ  ์ ์๋ ๊ตฌ์กฐ๋ก ๋ถ๋ฆฌํด ๋ด๋ ๊ฒ์ด ํจ์ฌ ๊น๋ํ๊ณ  ์ดํดํ๊ธฐ ์์ํ๋ค.
