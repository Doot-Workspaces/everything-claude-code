---
name: react-composition-patterns
description: "React composition patterns that scale — compound components, state management, context providers, React 19 changes. Use when refactoring boolean prop proliferation."
origin: ECC
---

# React Composition Patterns

Composition patterns for building flexible, maintainable React components. Avoid boolean prop proliferation by using compound components, lifting state, and composing internals.

## When to Use

- Refactoring components with many boolean props
- Building reusable component libraries
- Designing flexible component APIs
- Reviewing component architecture
- Working with compound components or context providers

## Rules by Priority

### 1. Component Architecture (HIGH)

**Avoid boolean props for behavior customization.**

Bad:
```tsx
<DataTable sortable filterable paginated searchable />
```

Good — use composition:
```tsx
<DataTable>
  <DataTable.Search />
  <DataTable.Filters />
  <DataTable.Body sortable />
  <DataTable.Pagination />
</DataTable>
```

**Use compound components for complex UI.** Share state through context, expose sub-components:
```tsx
<Select>
  <Select.Trigger />
  <Select.Options>
    <Select.Option value="a">Option A</Select.Option>
  </Select.Options>
</Select>
```

### 2. State Management (MEDIUM)

**Decouple state implementation.** The provider is the only place that knows how state is managed. Components consume a generic interface:

```tsx
interface StoreContext<T> {
  state: T;           // current data
  actions: Actions;   // mutations
  meta: Meta;         // loading, error, etc.
}
```

**Lift state into providers** when siblings need access:
```tsx
<FormProvider>
  <FormFields />      {/* reads state */}
  <FormActions />     {/* dispatches actions */}
  <FormValidation />  {/* reads meta */}
</FormProvider>
```

### 3. Implementation Patterns (MEDIUM)

**Create explicit variant components** instead of boolean modes:

Bad:
```tsx
<Button primary large outline />
```

Good:
```tsx
<PrimaryButton size="lg" variant="outline" />
```

**Use children over render props:**

Bad:
```tsx
<Card renderHeader={() => <h2>Title</h2>} renderFooter={() => <Actions />} />
```

Good:
```tsx
<Card>
  <Card.Header><h2>Title</h2></Card.Header>
  <Card.Body>Content</Card.Body>
  <Card.Footer><Actions /></Card.Footer>
</Card>
```

### 4. React 19 Changes (MEDIUM)

> React 19+ only. Skip if using React 18 or earlier.

- `ref` is a regular prop — no more `forwardRef` wrapper needed
- Use `use()` instead of `useContext()` for reading context
- These are API simplifications, not new patterns

## Protocol

1. **Identify the problem** -- read the component code and spot boolean prop proliferation, tightly coupled state, or render prop overuse
2. **Choose the pattern** -- select the appropriate composition pattern (compound components, provider, explicit variants)
3. **Propose the refactor** -- show before/after comparison with rationale
4. **Implement** -- produce production-ready refactored code
5. **Verify** -- run through the quick checklist below

## Output Format

```
Component: [ComponentName]
Current issue: [description of the anti-pattern]
Pattern applied: [compound components / provider / explicit variants / children composition]

Before:
  [brief code showing the anti-pattern]

After:
  [refactored code]

Rationale: [why this pattern fits better]
```

## Examples

**Example 1 -- Refactoring boolean props to compound components:**
```
Component: DataTable
Current issue: 5 boolean props (sortable, filterable, paginated, searchable, exportable)
Pattern applied: Compound components

Before:
  <DataTable sortable filterable paginated searchable exportable />

After:
  <DataTable>
    <DataTable.Search />
    <DataTable.Filters />
    <DataTable.Body sortable />
    <DataTable.Pagination />
    <DataTable.Export />
  </DataTable>

Rationale: Each feature is now opt-in via composition. Adding new features does not change the DataTable API.
```

**Example 2 -- Lifting state into a provider:**
```
Component: MultiStepForm
Current issue: Form state passed as props through 4 levels of nesting
Pattern applied: Provider with context

Before:
  <Form onSubmit={handleSubmit} values={values} errors={errors} onChange={handleChange}>
    <Step1 values={values} errors={errors} onChange={handleChange} />
    ...
  </Form>

After:
  <FormProvider onSubmit={handleSubmit}>
    <Step1 />  {/* reads state from context */}
    ...
  </FormProvider>

Rationale: Eliminates prop drilling. Each step reads only what it needs from context.
```

## Common Pitfalls

- Refactoring to compound components when a simple enum/string prop would suffice (over-engineering)
- Creating context providers for state that only one component uses (unnecessary abstraction)
- Using `forwardRef` in React 19+ projects where `ref` is a regular prop
- Replacing all render props with children composition even when render props provide necessary flexibility (e.g., headless UI libraries)
- Forgetting to memoize context values, causing unnecessary re-renders in all consumers

## Quick Checklist

Before shipping a component:
- [ ] No boolean props controlling behavior (use composition)
- [ ] State is decoupled from components (lives in provider)
- [ ] Complex components use compound pattern with sub-components
- [ ] Variants are explicit components or string props, not boolean flags
- [ ] Children used for composition, not render props
