# -
一个java初学者用于练手的记账本项目
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.YearMonth;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAdjusters;

    /**
     * 记账本专用时间工具类
     * 封装所有时间相关操作，方便复用
     */
    public class TimeUtils {
        // ========== 常用格式化器（常量，避免重复创建） ==========
        // 日期格式：yyyy-MM-dd（如2026-02-19）
        public static final DateTimeFormatter DATE_FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        // 日期时间格式：yyyy-MM-dd HH:mm:ss（如2026-02-19 15:30:20）
        public static final DateTimeFormatter DATETIME_FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        // 月份格式：yyyy-MM（如2026-02）
        public static final DateTimeFormatter MONTH_FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM");
        // 年份格式：yyyy（如2026）
        public static final DateTimeFormatter YEAR_FORMATTER = DateTimeFormatter.ofPattern("yyyy");

        // ========== 1. 获取当前时间（最常用） ==========
        /**
         * 获取当前日期（字符串，yyyy-MM-dd）
         */
        public static String getCurrentDateStr() {
            return LocalDate.now().format(DATE_FORMATTER);
        }

        /**
         * 获取当前日期时间（字符串，yyyy-MM-dd HH:mm:ss）
         */
        public static String getCurrentDateTimeStr() {
            return LocalDateTime.now().format(DATETIME_FORMATTER);
        }

        /**
         * 获取当前月份（字符串，yyyy-MM）
         */
        public static String getCurrentMonthStr() {
            return YearMonth.now().format(MONTH_FORMATTER);
        }

        // ========== 2. 时间计算（月度对比/年度总结用） ==========
        /**
         * 根据当前月份，获取上月（字符串，yyyy-MM）
         */
        public static String getLastMonthStr() {
            return YearMonth.now().minusMonths(1).format(MONTH_FORMATTER);
        }

        /**
         * 根据指定月份，获取上月（如传入2026-02，返回2026-01）
         * @param monthStr 格式：yyyy-MM
         */
        public static String getLastMonthStr(String monthStr) {
            YearMonth month = YearMonth.parse(monthStr, MONTH_FORMATTER);
            return month.minusMonths(1).format(MONTH_FORMATTER);
        }

        /**
         * 获取指定日期所在月的第一天（如传入2026-02-19，返回2026-02-01）
         * @param dateStr 格式：yyyy-MM-dd
         */
        public static String getFirstDayOfMonth(String dateStr) {
            LocalDate date = LocalDate.parse(dateStr, DATE_FORMATTER);
            return date.with(TemporalAdjusters.firstDayOfMonth()).format(DATE_FORMATTER);
        }

        /**
         * 获取指定日期所在月的最后一天（如传入2026-02-19，返回2026-02-28/29）
         * @param dateStr 格式：yyyy-MM-dd
         */
        public static String getLastDayOfMonth(String dateStr) {
            LocalDate date = LocalDate.parse(dateStr, DATE_FORMATTER);
            return date.with(TemporalAdjusters.lastDayOfMonth()).format(DATE_FORMATTER);
        }

        // ========== 3. 时间字符串解析（用户输入日期时用） ==========
        /**
         * 将字符串日期转为LocalDate对象（格式：yyyy-MM-dd）
         * 用于校验用户输入的日期是否合法
         */
        public static LocalDate parseDate(String dateStr) {
            try {
                return LocalDate.parse(dateStr, DATE_FORMATTER);
            } catch (Exception e) {
                throw new IllegalArgumentException("日期格式错误！正确格式：yyyy-MM-dd，如2026-02-19");
            }
        }

        /**
         * 将字符串月份转为YearMonth对象（格式：yyyy-MM）
         */
        public static YearMonth parseMonth(String monthStr) {
            try {
                return YearMonth.parse(monthStr, MONTH_FORMATTER);
            } catch (Exception e) {
                throw new IllegalArgumentException("月份格式错误！正确格式：yyyy-MM，如2026-02");
            }
        }

        public static LocalDateTime parseDateTime(String dateTimeStr){
            try {
                return LocalDateTime.parse(dateTimeStr,DATETIME_FORMATTER);
            } catch (Exception e){
                throw  new IllegalArgumentException("时间格式错误！正确格式：yyyy-MM-dd HH:mm:ss,如2026-2-21 10:59:45");
            }
        }

        // ========== 测试方法 ==========
        public static void main(String[] args) {
            // 测试获取当前时间
            System.out.println("当前日期：" + getCurrentDateStr());
            System.out.println("当前日期时间：" + getCurrentDateTimeStr());
            System.out.println("当前月份：" + getCurrentMonthStr());

            // 测试时间计算
            System.out.println("上月：" + getLastMonthStr());
            System.out.println("2026-03的上月：" + getLastMonthStr("2026-03"));
            System.out.println("2026-02-19所在月第一天：" + getFirstDayOfMonth("2026-02-19"));
            System.out.println("2026-02-19所在月最后一天：" + getLastDayOfMonth("2026-02-19"));

            // 测试解析
            System.out.println("解析日期：" + parseDate("2026-02-19"));
            System.out.println("解析月份：" + parseMonth("2026-02"));
        }
    }
import java.time.LocalDateTime;

public class Flow {
    private double amount;
    private LocalDateTime time;
    private String reason;
    public Flow() {
    }

    public Flow(double amount, String timeStr, String reason) {
        this.amount = amount;
        setTime(timeStr);
        this.reason = reason;
    }

    /**
     * 获取
     * @return amount
     */
    public double getAmount() {
        return amount;
    }

    /**
     * 设置
     * @param amount
     */
    public void setAmount(double amount) {
        this.amount = amount;
    }

    /**
     * 获取
     * @return LocalDateTime
     */
    public String getTimeStr() {
        return time.format(TimeUtils.DATETIME_FORMATTER);
    }

    /**
     * 设置
     * @param timeStr
     */
    public void setTime(String timeStr) {
        if (timeStr ==null || timeStr.trim().isEmpty()){
            throw new IllegalArgumentException("日常时间不能为空!");
        }
        this.time = TimeUtils.parseDateTime(timeStr);
    }

    /**
     * 获取
     * @return reason
     */
    public String getReason() {
        return reason;
    }

    /**
     * 设置
     * @param reason
     */
    public void setReason(String reason) {
        this.reason = reason;
    }

    /**
     * 获取
     * @return dayTime
     */
    public String getDayTime() {
        if (time == null){
            System.out.println("目前未设置时间，我将为您返回现在时间！");
            return TimeUtils.getCurrentDateStr();
        }
        return time.format(TimeUtils.DATE_FORMATTER);
    }

    /**
     * 获取
     * @return monthTime
     */
    public String getMonthTime() {
        if (time==null){
            System.out.println("目前并未设置时间，我将为您返回现在时间！");
            return TimeUtils.getCurrentMonthStr();
        }
        return time.format(TimeUtils.MONTH_FORMATTER);
    }


    public String toString() {
        return "Flow{amount = " + amount + ", Time = " + time + ", reason = " + reason +"}";
    }
}
import java.time.LocalDate;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

public class AccountBook {
    private ArrayList<Flow> transactionList=new ArrayList<>();
    private double totalAssets;
    private Double idealDayTransaction;
    private Double idealMonthTransaction;
    private double totalTransaction;
    private Map<String, Double> monthTransaction=new HashMap<>();
    private Map<String,Double> dayTransaction=new HashMap<>();

    public AccountBook() {
    }

    public AccountBook(ArrayList<Flow> transactionList, double totalAssets, Double idealDayTransaction, Double idealMonthTransaction,  double totalTransaction, Map<String, Double> monthTransaction, Map<String, Double> dayTransaction) {
        this.transactionList = transactionList;
        this.totalAssets = totalAssets;
        this.idealDayTransaction = idealDayTransaction;
        this.idealMonthTransaction = idealMonthTransaction;
        this.totalTransaction = totalTransaction;
        this.monthTransaction = monthTransaction;
        this.dayTransaction = dayTransaction;
    }

    /**
     * 获取
     * @return transactionList
     */
    public ArrayList<Flow> getTransactionList() {
        return transactionList;
    }

    /**
     * 设置
     * @param transactionList
     */
    public void setTransactionList(ArrayList<Flow> transactionList) {
        this.transactionList = transactionList;
    }

    /**
     * 获取
     * @return totalAssets
     */
    public double getTotalAssets() {
        return totalAssets;
    }

    /**
     * 设置
     * @param totalAssets
     */
    public void setTotalAssets(double totalAssets) {
        this.totalAssets = totalAssets;
    }

    /**
     * 获取
     * @return idealDayTransaction
     */
    public double getIdealDayTransaction() {
        return idealDayTransaction;
    }

    /**
     * 设置
     * @param idealDayTransaction
     */
    public void setIdealDayTransaction(Double idealDayTransaction) {
        this.idealDayTransaction = idealDayTransaction;
    }

    /**
     * 获取
     * @return idealMonthTransaction
     */
    public double getIdealMonthTransaction() {
        return idealMonthTransaction;
    }

    /**
     * 设置
     * @param idealMonthTransaction
     */
    public void setIdealMonthTransaction(Double idealMonthTransaction) {
        this.idealMonthTransaction = idealMonthTransaction;
    }



    public void addFlow(Flow flow) {
        transactionList.add(flow);
        totalAssets+= flow.getAmount();
        totalTransaction+= flow.getAmount();
        String currentDate= flow.getDayTime();
        String currentMonth=flow.getMonthTime();
        if (!dayTransaction.containsKey(currentDate)) {
            dayTransaction.put(currentDate, flow.getAmount());
        }else {
            double oldFlow=dayTransaction.get(currentDate);
            double newFlow=oldFlow+flow.getAmount();
            dayTransaction.put(currentDate,newFlow);
        }
        if (!monthTransaction.containsKey(currentMonth)) {
            monthTransaction.put(currentMonth, flow.getAmount());
        }else {
            double oldFlow=monthTransaction.get(currentMonth);
            double newFlow=oldFlow+flow.getAmount();
            monthTransaction.put(currentMonth,newFlow);
        }
    }

    /**
     * 获取
     * @return totalTransaction
     */
    public double getTotalTransaction() {
        return totalTransaction;
    }

    /**
     * 设置
     * @param totalTransaction
     */
    public void setTotalTransaction(double totalTransaction) {
        this.totalTransaction = totalTransaction;
    }

    /**
     * 获取
     * @return monthTransaction
     */
    public Map<String, Double> getMonthTransaction() {
        return monthTransaction;
    }

    /**
     * 设置
     * @param monthTransaction
     */
    public void setMonthTransaction(Map<String, Double> monthTransaction) {
        this.monthTransaction = monthTransaction;
    }

    /**
     * 获取
     * @return dayTransaction
     */
    public Map<String, Double> getDayTransaction() {
        return dayTransaction;
    }

    /**
     * 设置
     * @param dayTransaction
     */
    public void setDayTransaction(Map<String, Double> dayTransaction) {
        this.dayTransaction = dayTransaction;
    }

    public String toString() {
        return "AccountBook{transactionList = " + transactionList + ", totalAssets = " + totalAssets + ", idealDayTransaction = " + idealDayTransaction + ", idealMonthTransaction = " + idealMonthTransaction + ","+ ", totalTransaction = " + totalTransaction + ", monthTransaction = " + monthTransaction + ", dayTransaction = " + dayTransaction + "}";
    }

    public void compareToIdealDayTransaction(){
        if (idealDayTransaction==null){
            System.out.println("未设定每日理想流水，请重试");
            return;
        }
        String date=TimeUtils.getCurrentDateStr();
        double todayTransaction=dayTransaction.getOrDefault(date,0.0);
        if (todayTransaction>idealDayTransaction){
            System.out.println("您的今日流水已超过理想流水，非常棒！为成功挑战自己的您鼓掌！");
        } else if (todayTransaction==idealDayTransaction) {
            System.out.println("您的今日流水完美符合理想流水，非常棒！您一直再按您的计划有条不紊的进行着!");
        }else {
            System.out.println("您的今日流水少于理想流水，不过别灰心，人生常有意外，过去无法改变，请重鼓信心继续努力！");
        }
    }


    public void compareToIdealMonthTransaction(){
        if (idealMonthTransaction==null){
            System.out.println("未设定每月理想流水，请重试");
            return;
        }
        String month=TimeUtils.getCurrentMonthStr();
        double currentMonthTransaction=monthTransaction.getOrDefault(month,0.0);
        if (currentMonthTransaction>idealMonthTransaction){
            System.out.println("您的本月流水已超过理想流水，非常棒！为成功挑战自己的您鼓掌！");
        } else if (currentMonthTransaction==idealMonthTransaction) {
            System.out.println("您的本月流水完美符合理想流水，非常棒！您一直再按您的计划有条不紊的进行着!");
        }else {
            System.out.println("您的本月流水少于理想流水，不过别灰心，人生常有意外，过去无法改变，请重鼓信心继续努力！");
        }
    }


    public void checkCurrentDayTransaction(){
        double currentDayTrans=dayTransaction.getOrDefault(TimeUtils.getCurrentDateStr(),0.0);
        if (currentDayTrans>0) {
            System.out.printf("您今日流水是%.2f！非常棒！请您继续加油！\n",currentDayTrans);
        }else if (currentDayTrans==0) {
            System.out.printf("您今日流水是%.2f！稳稳当当的生活才是真！\n",currentDayTrans);
        }else {
            System.out.printf("您今日流水是%.2f！不过是些许风霜罢了！您一定能披荆斩棘，厚积薄发！\n",currentDayTrans);
        }
    }


    public void checkCurrentMonthTransaction(){
        double currentMonthTrans=monthTransaction.getOrDefault(TimeUtils.getCurrentMonthStr(),0.0);
        if (currentMonthTrans>0) {
            System.out.printf("您本月流水是%.2f！非常棒！请您继续加油！\n",currentMonthTrans);
        }else if (currentMonthTrans==0) {
            System.out.printf("您本月流水是%.2f！稳稳当当的生活才是真！\n",currentMonthTrans);
        }else {
            System.out.printf("您本月流水是%.2f！不过是些许风霜罢了！您一定能披荆斩棘，厚积薄发！\n",currentMonthTrans);
        }
    }


    public void checkTransactionDetails(){
        if (transactionList.isEmpty()){
            System.out.println("很抱歉，您暂时没有任何流水记录！");
            return;
        }
        for (Flow f:transactionList){
            System.out.printf("时间：%s 流水：%.2f 原因：%s\n",f.getTimeStr(),f.getAmount(),f.getReason());
        }
    }


    public void checkTotalAssets(){
        System.out.printf("您目前的总资产为%.2f\n",totalAssets);
    }

    public void checkTotalTransaction(){
        if (totalTransaction>0) {
            System.out.printf("您目前的总流水是%.2f！非常棒！请您继续加油！\n",totalTransaction);
        }else if (totalTransaction==0) {
            System.out.printf("您目前的总流水是%.2f!稳稳当当的生活才是真！\n",totalTransaction);
        }else {
            System.out.printf("您目前的总流水是%.2f!不过是些许风霜罢了！您一定能披荆斩棘，厚积薄发！\n",totalTransaction);

        }
    }


    public void removeFlow(Flow flow){
        if (!transactionList.contains(flow)){
            System.out.println("该记录并不在记账本中，请检查输入是否正确！");
            return;
        }
        transactionList.remove(flow);
        totalAssets-=flow.getAmount();
        totalTransaction-=flow.getAmount();
        dayTransaction.merge(flow.getDayTime(), -flow.getAmount(), Double::sum);
        monthTransaction.merge(flow.getMonthTime(), -flow.getAmount(), Double::sum);
        System.out.println("删除成功！");
    }


    public void searchFlowByDate(String date){
        try {
            TimeUtils.parseDate(date);
        } catch (IllegalArgumentException e) {
            System.out.println("日期格式错误，请重新输入。");
            return;
        }
        boolean isAvailable=false;
        for (Flow flow:transactionList){
            if (flow.getDayTime().equals(date)){
                System.out.println("时间："+flow.getTimeStr()+" 流水："+flow.getAmount()+" 原因："+flow.getReason());
                isAvailable=true;
            }
        }
        if (!isAvailable) {
            System.out.println("记账本中为找到该日记录，请重新确认！");
        }
    }

    public static void main(String[] args) {
        AccountBook ab=new AccountBook();
        ab.compareToIdealDayTransaction();
        ab.setIdealDayTransaction(1.1);
        Flow f=new Flow();
        f.setAmount(1.0);
        f.setTime(TimeUtils.getCurrentDateTimeStr());
        f.setReason("kindness");
        ab.addFlow(f);
        ab.compareToIdealDayTransaction();
        ab.checkCurrentDayTransaction();
        ab.checkCurrentMonthTransaction();
        ab.checkTransactionDetails();
        ab.searchFlowByDate(TimeUtils.getCurrentDateStr());
        ab.removeFlow(f);
        ab.checkTransactionDetails();
        ab.searchFlowByDate(TimeUtils.getCurrentDateStr());
    }
}
//1. 数据持久化（保存到文件）
//目前程序运行中的数据只在内存中，关闭后就会丢失。可以增加文件读写功能，让账本数据永久保存。
//
//方案：使用 ObjectOutputStream 将 AccountBook 对象序列化到文件，启动时读取；或者用文本文件（如CSV）保存每笔流水，便于查看和编辑。
//
//好处：下次打开程序还能看到历史记录。
//
//2. 增加修改/删除流水功能(finished)
//目前只能添加流水，无法修改或删除。可以扩展 AccountBook 类，提供以下方法：
//
//removeFlow(Flow flow)：从列表和汇总中移除指定流水。
//
//updateFlow(Flow oldFlow, Flow newFlow)：替换流水并重新计算汇总。
//主菜单中增加对应选项，通过流水ID或序号定位。
//
//3. 按分类统计（如支出/收入/原因）
//目前只按日/月汇总金额，可以增加按“原因”分类的统计，了解钱花在哪里。
//
//方案：在 Flow 中增加 category 字段（如餐饮、购物），AccountBook 中添加 Map<String, Double> categorySummary，在 addFlow 时更新。
//
//显示：增加“查看分类统计”菜单。
//
//4. 日期范围查询(finished)
//允许用户查询某段时间内的流水详情。
//
//方案：在 checkTransactionDetails 的基础上，增加起止日期输入，用 TimeUtils.parseDate 解析后，遍历 transactionList 筛选符合条件的流水打印。
//
//5. 输入校验增强
//金额限制：只允许保留两位小数（使用 BigDecimal 或正则校验）。
//
//原因长度限制：防止过长的输入。
//
//日期格式：允许用户自定义日期输入，并用 TimeUtils.parseDateTime 校验。
//
//6. 优化菜单交互
//提供“返回上一级”选项，避免每次都要回主菜单。
//
//在子菜单中增加“继续操作”或“退出”提示，更符合使用习惯。
//
//用数字或字母快速选择，减少输入错误。
//
//7. 代码重构，减少重复
//比较理想流水的方法中输出语句几乎一样，可以抽取一个私有方法，传入类型和金额。
//
//数字输入循环可封装成工具方法，例如 readDouble(String prompt)。
//
//8. 增加预算提醒
//设置每日/每月预算上限，当实际流水接近或超过时给出警告。
//
//在添加流水后检查并提示。
import java.util.Scanner;

public class AccountBookMain {
    private static final Scanner sc = new Scanner(System.in);
    private static AccountBook userAccountBook;

    public static void main(String[] args) {
        userAccountBook = new AccountBook();

        System.out.println("欢迎使用ILoveLJS的记账本程序！");

        // 首次设置总资产
        while (true) {
            System.out.println("首先，请输入您的总资产（数字，最多两位小数）：");
            String input = sc.nextLine().trim();
            if (input.matches("^-?\\d+(\\.\\d{1,2})?$")) {
                double assets = Double.parseDouble(input);
                userAccountBook.setTotalAssets(assets);
                System.out.println("记录成功！祝您使用愉快！");
                break;
            } else {
                System.out.println("输入无效，请重新输入数字（最多两位小数）！");
            }
        }


        // 主菜单循环
        while (true) {
            System.out.println("\n我将为您提供以下服务：");
            System.out.println("1. 记录您的日常流水");
            System.out.println("2. 记录您的理想流水");
            System.out.println("3. 记录您的总资产（更新）");
            System.out.println("4. 将您的实际流水与理想流水做比较");
            System.out.println("5. 查看您的具体流水详情");
            System.out.println("6. 查看您的基本理财信息");
            System.out.println("7. 删除流水");                 // 新增
            System.out.println("8. 按日期查询流水");            // 新增
            System.out.println("9. 退出本应用");                // 原7改为9
            System.out.print("请您输入序号进行操作：");

            String choice = sc.nextLine().trim();
            switch (choice) {
                case "1":
                    addDailyFlow();
                    break;
                case "2":
                    setIdealFlow();
                    break;
                case "3":
                    updateTotalAssets();
                    break;
                case "4":
                    compareFlow();
                    break;
                case "5":
                    userAccountBook.checkTransactionDetails();
                    break;
                case "6":
                    checkBasicInfo();
                    break;
                case "7":
                    deleteFlow();                               // 新增
                    break;
                case "8":
                    searchFlowByDate();                         // 新增
                    break;
                case "9":
                    System.out.println("感谢使用，再见！");
                    sc.close();
                    return;
                default:
                    System.out.println("无效选项，请重新输入。");
            }
        }
    }

    // ---------- 子功能模块（均支持输入 b 或 0 返回上一级）----------

    /** 记录日常流水（支持连续添加） */
    private static void addDailyFlow() {
        while (true) {
            System.out.println("\n--- 添加日常流水（输入 b 或 0 返回主菜单）---");

            Double amount = readAmount("请输入流水金额（正数为收入，负数为支出）：");
            if (amount == null) return;

            String timeStr = TimeUtils.getCurrentDateTimeStr();  // 默认当前时间

            String reason = readNonEmptyString("请输入流水原因（不超过50字符）：", 50);
            if (reason == null) return;

            Flow flow = new Flow();
            flow.setAmount(amount);
            flow.setTime(timeStr);
            flow.setReason(reason);

            userAccountBook.addFlow(flow);
            System.out.println("流水记录成功！");

            if (!askContinue("是否继续添加流水？(1:继续 / 其他:返回)")) {
                return;
            }
        }
    }

    /** 设置理想流水 */
    private static void setIdealFlow() {
        System.out.println("\n--- 设置理想流水（输入 b 或 0 返回）---");

        Double dayIdeal = readDoubleWithDefault("请设置每日理想流水（输入数字，直接回车跳过）：");
        if (dayIdeal == null) return;
        if (dayIdeal != Double.MIN_VALUE) {
            userAccountBook.setIdealDayTransaction(dayIdeal);
            System.out.println("每日理想流水设置成功！");
        }

        Double monthIdeal = readDoubleWithDefault("请设置每月理想流水（输入数字，直接回车跳过）：");
        if (monthIdeal == null) return;
        if (monthIdeal != Double.MIN_VALUE) {
            userAccountBook.setIdealMonthTransaction(monthIdeal);
            System.out.println("每月理想流水设置成功！");
        }
    }

    /** 更新总资产 */
    private static void updateTotalAssets() {
        System.out.println("\n--- 更新总资产（输入 b 或 0 返回）---");
        Double assets = readAmount("请输入新的总资产（数字，最多两位小数）：");
        if (assets == null) return;
        userAccountBook.setTotalAssets(assets);
        System.out.println("总资产更新成功！");
    }

    /** 比较流水与理想值 */
    private static void compareFlow() {
        while (true) {
            System.out.println("\n--- 比较流水（输入 b 或 0 返回主菜单）---");
            System.out.println("1. 比较今日流水与每日理想流水");
            System.out.println("2. 比较本月流水与每月理想流水");
            String choice = sc.nextLine().trim();
            if (choice.equals("b") || choice.equals("0")) return;

            switch (choice) {
                case "1":
                    userAccountBook.compareToIdealDayTransaction();
                    break;
                case "2":
                    userAccountBook.compareToIdealMonthTransaction();
                    break;
                default:
                    System.out.println("无效选择，请重新输入。");
                    continue;
            }
            if (!askContinue("是否继续比较？(1:继续 / 其他:返回)")) {
                return;
            }
        }
    }

    /** 查看基本信息（总资产优先） */
    private static void checkBasicInfo() {
        System.out.println("\n--- 基本信息 ---");
        userAccountBook.checkTotalAssets();
        userAccountBook.checkCurrentDayTransaction();
        userAccountBook.checkCurrentMonthTransaction();
        userAccountBook.checkTotalTransaction();
    }

    /** 删除流水（按时间字符串精确匹配） */
    private static void deleteFlow() {
        while (true) {
            System.out.println("\n--- 删除流水（输入 b 或 0 返回主菜单）---");
            System.out.println("请输入要删除的流水的时间（格式：yyyy-MM-dd HH:mm:ss）：");
            String timeStr = sc.nextLine().trim();
            if (timeStr.equals("b") || timeStr.equals("0")) return;

            // 校验格式
            try {
                TimeUtils.parseDateTime(timeStr);
            } catch (IllegalArgumentException e) {
                System.out.println("时间格式错误，请重新输入。");
                continue;
            }

            // 在交易列表中查找匹配的流水
            Flow targetFlow = null;
            for (Flow flow : userAccountBook.getTransactionList()) {
                if (flow.getTimeStr().equals(timeStr)) {
                    targetFlow = flow;
                    break;
                }
            }

            if (targetFlow == null) {
                System.out.println("未找到该时间对应的流水，请重新输入。");
                if (!askContinue("是否继续尝试删除？(1:继续 / 其他:返回)")) {
                    return;
                }
            } else {
                userAccountBook.removeFlow(targetFlow);
                System.out.println("流水已删除。");
                if (!askContinue("是否继续删除其他流水？(1:继续 / 其他:返回)")) {
                    return;
                }
            }
        }
    }

    /** 按日期查询流水 */
    private static void searchFlowByDate() {
        while (true) {
            System.out.println("\n--- 按日期查询流水（输入 b 或 0 返回主菜单）---");
            System.out.println("请输入日期（格式：yyyy-MM-dd）：");
            String date = sc.nextLine().trim();
            if (date.equals("b") || date.equals("0")) return;

            // 校验格式
            try {
                TimeUtils.parseDate(date);
            } catch (IllegalArgumentException e) {
                System.out.println("日期格式错误，请重新输入。");
                continue;
            }

            userAccountBook.searchFlowByDate(date);

            if (!askContinue("是否继续查询其他日期？(1:继续 / 其他:返回)")) {
                return;
            }
        }
    }

    // ---------- 输入辅助方法（均支持返回 null）----------

    private static Double readAmount(String prompt) {
        while (true) {
            System.out.print(prompt);
            String input = sc.nextLine().trim();
            if (input.equals("b") || input.equals("0")) {
                return null;
            }
            if (input.matches("^-?\\d+(\\.\\d{1,2})?$")) {
                return Double.parseDouble(input);
            }
            System.out.println("金额必须是数字，最多两位小数，请重新输入。");
        }
    }

    private static String readNonEmptyString(String prompt, int maxLength) {
        while (true) {
            System.out.print(prompt);
            String input = sc.nextLine().trim();
            if (input.equals("b") || input.equals("0")) {
                return null;
            }
            if (input.isEmpty()) {
                System.out.println("输入不能为空，请重新输入。");
            } else if (input.length() > maxLength) {
                System.out.println("输入长度不能超过 " + maxLength + " 字符，请重新输入。");
            } else {
                return input;
            }
        }
    }

    private static Double readDoubleWithDefault(String prompt) {
        while (true) {
            System.out.print(prompt);
            String input = sc.nextLine().trim();
            if (input.equals("b") || input.equals("0")) {
                return null;
            }
            if (input.isEmpty()) {
                return Double.MIN_VALUE; // 表示跳过
            }
            try {
                return Double.parseDouble(input);
            } catch (NumberFormatException e) {
                System.out.println("输入无效，请重新输入数字或直接回车跳过。");
            }
        }
    }

    private static boolean askContinue(String prompt) {
        System.out.print(prompt);
        String choice = sc.nextLine().trim();
        return choice.equals("1");
    }
}
