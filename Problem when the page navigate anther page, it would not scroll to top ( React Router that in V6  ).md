# Problem: when the page navigate anther page, it would not scroll to top ( React Router that in V6  )

## fix

add the scroll behavior in the `useNaviagte` hook

for example

```typescript
import { useNavigate } from "react-router-dom";
import { NavigateOptions } from "react-router-dom";

/**
 * Navigate to the top of a page so that the scroll position isn't persisted between pages.
 * @see Use this instead of React Dom's build-in  {@link useNavigate}
 * @returns
 */
const useNavigateToTop = () => {
	const navigate = useNavigate();

	const navigateAndReset = (to: string, options?: NavigateOptions) => {
		navigate(to, { ...options });
		window.scrollTo({
			top: 0,
			left: 0,
			//@ts-ignore
			behavior: "instant",
		});
	};

	return navigateAndReset;
};

export { useNavigateToTop };

```

