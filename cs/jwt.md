## JWT(Json Web Token)

쿠키와 세션과 같은 인터넷 표준 인증 방식 중 하나이다.
인증에 필요한 정보들을 토큰에 담아 암호화시켜 사용한다.

로그인 할 때의 인증 플로우로 JWT를 이해해보자.

1. 유저가 로그인 정보를 입력 후 로그인 요청을 보냄
2. 서버는 로그인 정보가 유효한지 데이터베이스에서 확인
3. 유효한 경우, 토큰을 발행해서 클라에게 전송
   - 토큰을 발행할 때, 페이로드에는 유저를 식별할 수 있는 정보를 담아야 함
   - 자체적으로 비밀키를 생성해서, 토큰에 서명을 해야함
4. 이후 클라에서는 헤더에 토큰을 담아 서버로 요청을 보냄
5. 서버는 헤더에 담긴 토큰이 유효한지 검증함
   - 토큰을 발행할 때 사용했던 비밀키로 검증하기에, 비밀키가 절대 공개되어서는 안됨

#### Access Token

- **인증 및 권한 부여**: Access Token은 사용자가 시스템이나 애플리케이션에 접근할 수 있는 권한을 가지고 있음을 증명한다. 서버는 이 토큰을 검증하여 사용자의 요청을 승인한다.
- **자가 수용적(Self-contained)**: 특히 JSON Web Token (JWT) 형식의 Access Token은 사용자에 대한 정보(예: 사용자 ID, 권한, 토큰의 유효 시간 등)를 토큰 자체에 포함할 수 있다.
- **유효 기간**: Access Token은 보안상의 이유로 일반적으로 짧은 유효 기간을 가진다. 유효 기간이 지난 토큰은 자동으로 만료되어 더 이상 사용할 수 없다.
- **HTTP 헤더를 통한 전송**: 클라이언트는 API 요청을 할 때 HTTP 헤더에 Access Token을 포함시켜 서버로 전송한다. 서버는 이 토큰을 검증하고 요청된 작업을 수행한다.

#### Refresh Token

- **긴 유효 기간**: Refresh Token은 Access Token보다 훨씬 긴 유효 기간을 가지고 있다. 이는 사용자가 자주 로그인을 반복하지 않아도 되게 하며, 사용자의 편의성을 제공한다.
- **보안**: Refresh Token은 보안이 중요한 정보를 저장하거나 직접적으로 리소스에 접근하는 데 사용되지 않는다. 대신, 만료된 Access Token을 새로 발급받는 데 사용된다.
- **재발급 절차**: 사용자의 Access Token이 만료되면, 클라이언트는 Refresh Token을 사용하여 인증 서버에 새로운 Access Token을 요청한다. 서버는 Refresh Token을 검증한 후, 새로운 Access Token을 발급한다.
- **재발급 시 Refresh Token Rotation**: 보안을 강화하기 위해, 새로운 Access Token을 발급받을 때마다 새로운 Refresh Token도 함께 발급받을 수 있다. 이전의 Refresh Token은 무효화되며, 이 과정을 Refresh Token Rotation이라고 한다.

#### References

- **Refresh Token Rotation**: https://auth0.com/docs/secure/tokens/refresh-tokens/refresh-token-rotation
- **Refresh Token 을 DB에 저장해야 하는 이유**: https://betterprogramming.pub/should-we-store-tokens-in-db-af30212b7f22
  .
