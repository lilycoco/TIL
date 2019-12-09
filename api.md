# API


``` typescript
const bookListGetEpic: Epic<Action, RootState> = (action$, state) =>
  action$.ofType(bookListGetAction.types.request)
    .debounceTime(275) // 毎回変更があるたびにレスポンスを返すと処理速度が遅くなるので　debounceTime() を設定する
    .map(action =>
      ajaxAction.request({
        method: "get",
        url: `/api/admin/orgs/${state.getState().admin.selectedOrg.name}/books`,
        body: {categories: [state.getState().admin.selectedOrg.name === "givery" ? 1 : 2], ...action.payload},
        success: bookListGetAction.success,
      })
    );
```
