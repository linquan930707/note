import java.text.SimpleDateFormat;
import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAdjusters;
import java.util.*;
import java.util.concurrent.*;

/**
 * Created by lusq on 2018/11/7.
 */
public class DateTimeUtil {

    private static final ThreadLocal<SimpleDateFormat> dateFormatterYYYYMMDD = new ThreadLocal<SimpleDateFormat>(){
        @Override
        protected SimpleDateFormat initialValue() {
            return  new SimpleDateFormat("yyyy-MM-dd");
        }
    };
    private static final ThreadLocal<SimpleDateFormat> dateFormatterYYYYMMDDHHmmss = new ThreadLocal<SimpleDateFormat>(){
        @Override
        protected SimpleDateFormat initialValue() {
            return  new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        }
    };

    public static String getDateFormatterYYYYMMDD() {
        return dateFormatterYYYYMMDD.get().toPattern();
    }

    public static String getDateFormatterYYYYMMDDHHmmss() {
        return dateFormatterYYYYMMDDHHmmss.get().toPattern();
    }

    /**
     * 获取现在时间
     * @return
     */
    public static Date getNowDate(){
        LocalDateTime localDateTime  = LocalDateTime.now();
        Instant instant = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
        return Date.from(instant);
    }

    /**
     * date 转 LocalDateTime
     * @param date
     * @return
     */
    public static LocalDateTime convertDateToLDT(Date date) {
        return LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
    }

    /**
     * LocalDateTime 转 date
     * @param time
     * @return
     */
    public static Date convertLDTToDate(LocalDateTime time) {
        return Date.from(time.atZone(ZoneId.systemDefault()).toInstant());
    }

    /**
     * 获取指定时间的指定格式
     * @param time  日期
     * @param pattern  格式  例如：yyyy-MM-dd
     * @return
     */
    public static String formatTime(LocalDateTime time,String pattern) {
        return time.format(DateTimeFormatter.ofPattern(pattern));
    }

    /**
     * 获取当前日期的指定格式
     * @param pattern
     * @return
     */
    public static String formatNow(String pattern) {
        return  formatTime(LocalDateTime.now(), pattern);
    }


    /**
     * date 转为指定格式
     * @param date
     * @param pattern
     * @return
     */
    public static String dateFormat(Date date,String pattern){
        LocalDateTime localDateTime = convertDateToLDT(date);
        return formatTime(localDateTime,pattern);
    }

    /**
     *  long类型的timestamp转为LocalDateTime
     * @param timestamp
     * @return
     */
    public static LocalDateTime getDateTimeOfTimestamp(long timestamp) {
        Instant instant = Instant.ofEpochMilli(timestamp);
        ZoneId zone = ZoneId.systemDefault();
        return LocalDateTime.ofInstant(instant, zone);
    }

    /**
     *  long类型的timestamp转为date
     *  @param timestamp
     *  @return
     */
    public static Date getDateOfTimstamp(long timestamp){
       return  convertLDTToDate(getDateTimeOfTimestamp(timestamp));
    }

    /**
     * 本周日期集合
     * @return
     */
    public static List<String> weekStage(){
        LocalDateTime localDateTime = LocalDateTime.now();
        LocalDateTime monday = localDateTime.with(DayOfWeek.MONDAY);
        LocalDateTime sunday = localDateTime.with(DayOfWeek.SUNDAY);
        return modifyDate(monday,sunday);
    }


    /**
     * 本周 周一、周日
     */
    public static Map<String,String> getWeekStartDayAndEndDay(){
        Map<String,String>dataMap = new HashMap<String,String>();
        LocalDateTime localDateTime = LocalDateTime.now();
        String monday = formatTime(localDateTime.with(DayOfWeek.MONDAY),getDateFormatterYYYYMMDD());
        String sunday = formatTime(localDateTime.with(DayOfWeek.SUNDAY),getDateFormatterYYYYMMDD());
        dataMap.put("startDate",monday);
        dataMap.put("endDate",sunday);
        return dataMap;
    }

    /**
     * 获取本月第一天和最后一天
     * @return
     */
    public static Map<String,String>getMonthStartDayAndEndDay(){
        //
        Map<String,String>dataMap = new HashMap<String,String>();
        LocalDateTime localDateTime = LocalDateTime.now();
        String monday = formatTime(localDateTime.with(TemporalAdjusters.firstDayOfMonth()),getDateFormatterYYYYMMDD());
        String sunday = formatTime(localDateTime.with(TemporalAdjusters.lastDayOfMonth()),getDateFormatterYYYYMMDD());
        dataMap.put("startDate",monday);
        dataMap.put("endDate",sunday);
        return dataMap;
    }


    /**
     * 获取近几个月的开始时间和结束时间
     * @param month  前几个月
     * @return
     */
    public static Map<String,String>getNearMontyStartDayEndDay(Integer month){
        Map<String,String>dataMap = new HashMap<String,String>();
        LocalDateTime localDateTime = LocalDateTime.now();
        LocalDateTime threeMonthAgeDay = localDateTime.minusMonths(month);
        String startDay = formatTime(threeMonthAgeDay.with(TemporalAdjusters.firstDayOfMonth()),getDateFormatterYYYYMMDD());
        String endDay = formatTime(localDateTime.with(TemporalAdjusters.lastDayOfMonth()),getDateFormatterYYYYMMDD());
        dataMap.put("startDate",startDay);
        dataMap.put("endDate",endDay);
        return dataMap;
    }

    /**
     * 获取本年第一天和最后一天
     * @return
     */
    public static Map<String,String>getYearStartDayAndEndDay(){
        Map<String,String>dataMap = new HashMap<String,String>();
        LocalDateTime localDateTime = LocalDateTime.now();
        String monday = formatTime(localDateTime.with(TemporalAdjusters.firstDayOfYear()),getDateFormatterYYYYMMDD());
        String sunday = formatTime(localDateTime.with(TemporalAdjusters.lastDayOfYear()),getDateFormatterYYYYMMDD());
        dataMap.put("startDate",monday);
        dataMap.put("endDate",sunday);
        return dataMap;
    }

    /**
     * 查询中间开始时间和结束时间之间的日期集合
     * @param startDate
     * @param endDate
     * @return
     */
    public static List<String>modifyDate(LocalDateTime startDate,LocalDateTime endDate){
        List<String>dataList= new ArrayList<>();
        while (startDate.isBefore(endDate)){
            dataList.add(formatTime(startDate,getDateFormatterYYYYMMDD()));
            startDate=startDate.plusDays(1);
        }
        dataList.add(formatTime(endDate,DateTimeUtil.getDateFormatterYYYYMMDD()));
        return dataList;
    }









    public static void main(String[]args){
     /*   Map<String, String> yearStartDayAndEndDay = getNearThreeMonty();
        System.out.println(yearStartDayAndEndDay.get("startDate"));
        System.out.println(yearStartDayAndEndDay.get("endDate"));*/


     /*
        ExecutorService executorService = new ThreadPoolExecutor(1, 1,
                0L, TimeUnit.MILLISECONDS,
                new LinkedBlockingQueue<Runnable>(1024), new ThreadPoolExecutor.AbortPolicy());

        executorService.execute(new Runnable() {
            @Override
            public void run() {
                while(true){
                    Map<String,String>maps = getYearStartDayAndEndDay();
                    System.out.println(maps.get("endDate"));
                }
            }
        });
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                while(true){
                    Map<String,String>maps = getYearStartDayAndEndDay();
                    System.out.println(maps.get("endDate"));
                }

            }
        });*/


        Map<String, String> yearStartDayAndEndDay = getYearStartDayAndEndDay();
        System.out.println(yearStartDayAndEndDay.get("startDate"));
        System.out.println(yearStartDayAndEndDay.get("endDate"));

    }



}
