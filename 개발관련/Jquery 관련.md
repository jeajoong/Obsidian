

jquery input 값 변경 실시간 감지

//예전 jQuery라면 on이 아니라 bind나 live 
$("#text").on("propertychange change keyup paste input", function() {
    var currentVal = $(this).val();
    if(currentVal == oldVal) {
        return;
    }
    oldVal = currentVal;
    alert("changed!");
});