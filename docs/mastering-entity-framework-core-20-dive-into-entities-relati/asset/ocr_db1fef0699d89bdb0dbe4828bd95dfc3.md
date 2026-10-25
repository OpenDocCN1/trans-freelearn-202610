```
<div class="form-group">
    <label class="col-md-2 control-label">Header Image</label>
    <div class="col-md-10">
        <input type="file" id="headerImage" name="headerImage" />
    </div>
</div>
// Code removed for brevity
<div class="form-group">
    <label asp-for="PublishedDateTime" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="PublishedDateTime" class="form-control datepicker" />
        <span asp-validation-for="PublishedDateTime" class="text-danger"></span>
    </div>
</div>
// Code removed for brevity
<div class="form-group">
    <label asp-for="TagIds" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <select asp-for="TagIds" class="form-control" asp-items="ViewBag.TagIds" multiple="multiple"></select>
    </div>
</div>
// Code removed for brevity
</div>
</form>
// Code removed for brevity
@section Scripts {
    // Code removed for brevity
    <script>
        // Code removed for brevity
        $('.datepicker').datetimepicker();
    </script>
}
```

We have seen the datepicker incorporation and the file upload input type included in the Post view. In the next section, we will see how transactions could be maintained between multiple technologies.