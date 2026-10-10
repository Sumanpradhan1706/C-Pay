# Phone number data source

Authentication is email-only: the active onboarding flow verifies email and passes an empty `phoneNumber` to wallet creation. There is no active phone OTP flow.

Phone numbers can still appear through legacy profile data:

- `EmailVerificationScreen` reads `users.phone_number` for an existing profile and passes it to `RestoreWalletScreen`.
- `RestoreWalletScreen` copies that existing value to AsyncStorage under `phone_number` when restoring a wallet.
- `ProfileSetupScreen` accepts the legacy `phoneNumber` route parameter and, when it is present and is not the development phone, writes it to AsyncStorage `phone_number` and `users.phone_number`.
- `ConfirmPINScreen` accepts the same route parameter and may initially persist it to `users.phone_number` while creating a wallet.

The normal email signup route supplies an empty phone value, so it does not populate `users.phone_number`. `phone_number` should therefore be treated as legacy profile data, not as a verified or currently collected authentication factor. The `user_phone` AsyncStorage key has no writer in `App/src`.

The app brand primary remains `#2563EB`, defined in `App/src/constants/theme.ts`; no other brand primary is defined under `App/src`.

## Original issue #15 verification

The requested dead-code deletions are present on `main`: `App/src/constants/config.ts`, `App/src/utils/simCard.ts`, and `App/src/utils/resetApp.ts` are absent. `App/src/constants/` contains only `theme.ts`. The `relayer_idempotency_keys` table is retained and used by `relayer-service/server.js` at line 1247.
