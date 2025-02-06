# Add a Subcategory Listing in Catalyst

## Setup

**Run dev server:**

```shell
pnpm run dev
```

## Add the Subcategory List Logic

**`components/subcategory-list/component-data.ts`:**

```javascript
import { cache } from 'react';

import { getSessionCustomerAccessToken } from '~/auth';
import { client } from '~/client';
import { graphql, VariablesOf } from '~/client/graphql';
import { revalidate } from '~/client/revalidate-target';

const SubcategoriesQuery = graphql(
  `
    query SubcategoriesQuery($categoryId: Int!) {
      site {
        categoryTree(rootEntityId: $categoryId) {
          entityId
          children {
            entityId
            name
            path
            image {
              altText
              url: urlTemplate(lossy: true)
            }
            productCount
          }
        }
      }
    }
  `
);

export const getSubcategories = cache(
  async (variables: VariablesOf<typeof SubcategoriesQuery>) => {
    const customerAccessToken = await getSessionCustomerAccessToken();

    const response = await client.fetch({
      document: SubcategoriesQuery,
      variables,
      customerAccessToken,
      fetchOptions: customerAccessToken ? { cache: 'no-store' } : { next: { revalidate } },
    });

    return response.data.site.categoryTree[0]?.children ?? [];
  }
);
```

**`app/[locale]/(default)/(faceted)/category/[slug]/page.tsx`:**

```javascript
import { getSubcategories } from '~/components/subcategory-list/component-data';

...

export default async function Category(props: Props) {
  // START MODIFIED CODE
  const { locale, slug } = await props.params;
  // END MODIFIED CODE

  ...

  // START NEW CODE
  const subcategories = await getSubcategories({
    categoryId: Number(slug),
  });

  if (subcategories.length > 0) {
    return <div className="@container text-2xl p-8">
      Subcategory List Placeholder
    </div>;
  }
  // END NEW CODE

  return (
    ...
  );
}
```

## Add the Subcategory Listing Component

**`components/subcategory-list/index.tsx`:**

**Step 1:**

```javascript
import { CSSProperties } from 'react';

import { Card } from '@/vibes/soul/primitives/card';

import { getSubcategories } from './component-data';

export const SubcategoryList = (
  {
    title,
    subcategories
  }: {
    title: string | null,
    subcategories: Awaited<ReturnType<typeof getSubcategories>>,
  }
) => {
  return (

  );
};
```

**Step 2:**

```javascript
export const SubcategoryList = (
  ...
) => {
  return (
    // START NEW CODE
    <div className="@container 
      mx-auto max-w-screen-2xl px-4 py-10 
      @xl:px-6 @xl:py-14 @4xl:px-8 @4xl:py-12"
    >
      <h1 className="font-heading 
        text-3xl font-medium leading-none 
        @lg:text-4xl @2xl:text-5xl
        text-contrast-400"
      >
          {title}
      </h1>
      <div className="w-full @container my-4">
        <div className="mx-auto 
          grid grid-cols-1 gap-4 
          @lg:grid-cols-2 @2xl:grid-cols-3"
        >
          {subcategories.map((subcategory) => (
            
          ))}
        </div>
      </div>
    </div>
    // END NEW CODE
  );
};
```

**Step 3:**

```javascript
export const SubcategoryList = (
  ...
) => {
  return (
    <div ...>
      ...
      <div ...>
        <div ...>
          {subcategories.map((subcategory) => (
            {/* START NEW CODE */}
            <Card
              className=''
              href={subcategory.path}
              image={(!subcategory.image) ? undefined : {
                src: subcategory.image?.url ?? '',
                alt: subcategory.image?.altText ?? '',
              }}
              key={subcategory.entityId}
              title={`${subcategory.name} (${subcategory.productCount})`}
            />
            {/* END NEW CODE */}
          ))}
        </div>
      </div>
    </div>
  );
};
```

**`app/[locale]/(default)/(faceted)/category/[slug]/page.tsx`:**

```javascript
import { SubcategoryList } from '~/components/subcategory-list';

...

if (subcategories.length > 0) {
    // START MODIFIED CODE
    return <SubcategoryList
      subcategories={subcategories}
      title={await getTitle(props)}
    />;
    // END MODIFIED CODE
  }
```

## Create Style Variables

**`components/subcategory-list/index.tsx`:**

```javascript
export const SubcategoryList = (
  ...
) => {
  return (
    <div ...>
      ...
      <div ...>
        <div className="..."

          {/* START NEW CODE */}
          style={{
            '--card-light-text': 'hsl(0 0% 0%)',
            '--card-border-radius': '0',
          } as CSSProperties}
          {/* END NEW CODE */}
        >
          {subcategories.map((subcategory) => (
            ...
          ))}
        </div>
      </div>
    </div>
  );
};
```

**`app/globals.css`:**

```css
:root {
  ...

  --subcategory-list-heading: hsl(96 25% 68%);
  --subcategory-list-light-text: hsl(96 100% 30%);
  --subcategory-list-border-radius: 2rem;
}
```

**`components/subcategory-list/index.tsx`:**

```javascript
export const SubcategoryList = (
  ...
) => {
  return (
    <div ...>
      ...
      <div ...>
        <div className="..."

          {/* START MODIFIED CODE */}
          style={{
            '--card-light-text': 'var(--subcategory-list-light-text)',
            '--card-border-radius': 'var(--subcategory-list-border-radius)',
          } as CSSProperties}
          {/* END MODIFIED CODE */}
        >
          {subcategories.map((subcategory) => (
            ...
          ))}
        </div>
      </div>
    </div>
  );
};
```

**`tailwind.config.ts`:**

```javascript
const config = {
  ...
  theme: {
    extend: {
      colors: {
        // START NEW CODE 
        subcategoryListHeading: 'var(--subcategory-list-heading)',
        // END NEW CODE
        primary: {
          ...
        },
        ...
      },
      ...
    },
  },
  ...
};
```

**`components/subcategory-list/index.tsx`:**

```javascript
export const SubcategoryList = (
  ...
) => {
  return (
    <div ...>
      {/* START MODIFIED CODE */}
      <h1 className="font-heading 
        text-3xl font-medium leading-none 
        @lg:text-4xl @2xl:text-5xl
        text-subcategoryListHeading"
      >
      {/* END MODIFIED CODE */}
          {title}
      </h1>

      ...
    </div>
  );
};
```
