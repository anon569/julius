cmake_minimum_required(VERSION 2.6)
project(julius C)

find_package(Git)

set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${CMAKE_SOURCE_DIR}/cmake/")

if (CMAKE_VERSION VERSION_LESS 3.1)
    if (CMAKE_COMPILER_IS_GNUCC)
        set(CMAKE_C_FLAGS "-std=gnu99 ${CMAKE_C_FLAGS}")
    endif()
else()
    set(CMAKE_C_STANDARD 99)
endif()

if (CMAKE_COMPILER_IS_GNUCC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Werror=implicit-function-declaration")

    if (CMAKE_C_COMPILER_VERSION VERSION_GREATER 5.0)
        set(CMAKE_C_FLAGS "-Werror=incompatible-pointer-types -Werror=int-conversion -Wstrict-prototypes ${CMAKE_C_FLAGS}")
    endif()
endif()

set(SMK_FILES
    ext/libsmacker/smacker.c
    ext/libsmacker/smk_bitstream.c
    ext/libsmacker/smk_hufftree.c
)
if(EXISTS "${CMAKE_SOURCE_DIR}/.git")
  execute_process(
    COMMAND git describe --long --dirty --abbrev=10 --tags
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_COMMIT_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
  )
else(EXISTS "${CMAKE_SOURCE_DIR}/.git")
  set(GIT_COMMIT_HASH "")
endif(EXISTS "${CMAKE_SOURCE_DIR}/.git")

configure_file(${PROJECT_SOURCE_DIR}/gen/version.h.in ${PROJECT_SOURCE_DIR}/src/platform/version.h)

set(PLATFORM_FILES
    ${PROJECT_SOURCE_DIR}/src/platform/julius.c
    ${PROJECT_SOURCE_DIR}/src/platform/cursor.c
    ${PROJECT_SOURCE_DIR}/src/platform/keyboard_input.c
    ${PROJECT_SOURCE_DIR}/src/platform/log.c
    ${PROJECT_SOURCE_DIR}/src/platform/screen.c
    ${PROJECT_SOURCE_DIR}/src/platform/sound_device.c
)

set(CORE_FILES
    ${PROJECT_SOURCE_DIR}/src/core/buffer.c
    ${PROJECT_SOURCE_DIR}/src/core/calc.c
    ${PROJECT_SOURCE_DIR}/src/core/dir.c
    ${PROJECT_SOURCE_DIR}/src/core/file.c
    ${PROJECT_SOURCE_DIR}/src/core/image.c
    ${PROJECT_SOURCE_DIR}/src/core/io.c
    ${PROJECT_SOURCE_DIR}/src/core/lang.c
    ${PROJECT_SOURCE_DIR}/src/core/random.c
    ${PROJECT_SOURCE_DIR}/src/core/string.c
    ${PROJECT_SOURCE_DIR}/src/core/time.c
    ${PROJECT_SOURCE_DIR}/src/core/zip.c
)

set(BUILDING_FILES
    ${PROJECT_SOURCE_DIR}/src/building/animation.c
    ${PROJECT_SOURCE_DIR}/src/building/barracks.c
    ${PROJECT_SOURCE_DIR}/src/building/building.c
    ${PROJECT_SOURCE_DIR}/src/building/building_state.c
    ${PROJECT_SOURCE_DIR}/src/building/construction.c
    ${PROJECT_SOURCE_DIR}/src/building/construction_building.c
    ${PROJECT_SOURCE_DIR}/src/building/construction_clear.c
    ${PROJECT_SOURCE_DIR}/src/building/construction_warning.c
    ${PROJECT_SOURCE_DIR}/src/building/count.c
    ${PROJECT_SOURCE_DIR}/src/building/destruction.c
    ${PROJECT_SOURCE_DIR}/src/building/dock.c
    ${PROJECT_SOURCE_DIR}/src/building/figure.c
    ${PROJECT_SOURCE_DIR}/src/building/government.c
    ${PROJECT_SOURCE_DIR}/src/building/granary.c
    ${PROJECT_SOURCE_DIR}/src/building/house.c
    ${PROJECT_SOURCE_DIR}/src/building/house_evolution.c
    ${PROJECT_SOURCE_DIR}/src/building/house_population.c
    ${PROJECT_SOURCE_DIR}/src/building/house_service.c
    ${PROJECT_SOURCE_DIR}/src/building/industry.c
    ${PROJECT_SOURCE_DIR}/src/building/list.c
    ${PROJECT_SOURCE_DIR}/src/building/maintenance.c
    ${PROJECT_SOURCE_DIR}/src/building/market.c
    ${PROJECT_SOURCE_DIR}/src/building/menu.c
    ${PROJECT_SOURCE_DIR}/src/building/model.c
    ${PROJECT_SOURCE_DIR}/src/building/properties.c
    ${PROJECT_SOURCE_DIR}/src/building/storage.c
    ${PROJECT_SOURCE_DIR}/src/building/warehouse.c
)
set(CITY_FILES
    ${PROJECT_SOURCE_DIR}/src/city/buildings.c
    ${PROJECT_SOURCE_DIR}/src/city/culture.c
    ${PROJECT_SOURCE_DIR}/src/city/data.c
    ${PROJECT_SOURCE_DIR}/src/city/data_private.c
    ${PROJECT_SOURCE_DIR}/src/city/emperor.c
    ${PROJECT_SOURCE_DIR}/src/city/entertainment.c
    ${PROJECT_SOURCE_DIR}/src/city/festival.c
    ${PROJECT_SOURCE_DIR}/src/city/figures.c
    ${PROJECT_SOURCE_DIR}/src/city/finance.c
    ${PROJECT_SOURCE_DIR}/src/city/gods.c
    ${PROJECT_SOURCE_DIR}/src/city/health.c
    ${PROJECT_SOURCE_DIR}/src/city/houses.c
    ${PROJECT_SOURCE_DIR}/src/city/labor.c
    ${PROJECT_SOURCE_DIR}/src/city/map.c
    ${PROJECT_SOURCE_DIR}/src/city/message.c
    ${PROJECT_SOURCE_DIR}/src/city/migration.c
    ${PROJECT_SOURCE_DIR}/src/city/military.c
    ${PROJECT_SOURCE_DIR}/src/city/mission.c
    ${PROJECT_SOURCE_DIR}/src/city/population.c
    ${PROJECT_SOURCE_DIR}/src/city/ratings.c
    ${PROJECT_SOURCE_DIR}/src/city/resource.c
    ${PROJECT_SOURCE_DIR}/src/city/sentiment.c
    ${PROJECT_SOURCE_DIR}/src/city/sound.c
    ${PROJECT_SOURCE_DIR}/src/city/trade.c
    ${PROJECT_SOURCE_DIR}/src/city/victory.c
    ${PROJECT_SOURCE_DIR}/src/city/view.c
    ${PROJECT_SOURCE_DIR}/src/city/warning.c
)
set(EMPIRE_FILES
    ${PROJECT_SOURCE_DIR}/src/empire/city.c
    ${PROJECT_SOURCE_DIR}/src/empire/empire.c
    ${PROJECT_SOURCE_DIR}/src/empire/object.c
    ${PROJECT_SOURCE_DIR}/src/empire/trade_prices.c
    ${PROJECT_SOURCE_DIR}/src/empire/trade_route.c
)
set(FIGURE_FILES
    ${PROJECT_SOURCE_DIR}/src/figure/action.c
    ${PROJECT_SOURCE_DIR}/src/figure/combat.c
    ${PROJECT_SOURCE_DIR}/src/figure/enemy_army.c
    ${PROJECT_SOURCE_DIR}/src/figure/figure.c
    ${PROJECT_SOURCE_DIR}/src/figure/formation.c
    ${PROJECT_SOURCE_DIR}/src/figure/formation_enemy.c
    ${PROJECT_SOURCE_DIR}/src/figure/formation_herd.c
    ${PROJECT_SOURCE_DIR}/src/figure/formation_layout.c
    ${PROJECT_SOURCE_DIR}/src/figure/formation_legion.c
    ${PROJECT_SOURCE_DIR}/src/figure/image.c
    ${PROJECT_SOURCE_DIR}/src/figure/movement.c
    ${PROJECT_SOURCE_DIR}/src/figure/name.c
    ${PROJECT_SOURCE_DIR}/src/figure/phrase.c
    ${PROJECT_SOURCE_DIR}/src/figure/properties.c
    ${PROJECT_SOURCE_DIR}/src/figure/route.c
    ${PROJECT_SOURCE_DIR}/src/figure/service.c
    ${PROJECT_SOURCE_DIR}/src/figure/sound.c
    ${PROJECT_SOURCE_DIR}/src/figure/trader.c
)
set(FIGURETYPE_FILES
    ${PROJECT_SOURCE_DIR}/src/figuretype/animal.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/cartpusher.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/crime.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/docker.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/enemy.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/entertainer.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/maintenance.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/market.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/migrant.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/missile.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/native.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/service.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/soldier.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/trader.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/wall.c
    ${PROJECT_SOURCE_DIR}/src/figuretype/water.c
)
set(GAME_FILES
    ${PROJECT_SOURCE_DIR}/src/game/animation.c
    ${PROJECT_SOURCE_DIR}/src/game/difficulty.c
    ${PROJECT_SOURCE_DIR}/src/game/file.c
    ${PROJECT_SOURCE_DIR}/src/game/file_io.c
    ${PROJECT_SOURCE_DIR}/src/game/game.c
    ${PROJECT_SOURCE_DIR}/src/game/mission.c
    ${PROJECT_SOURCE_DIR}/src/game/orientation.c
    ${PROJECT_SOURCE_DIR}/src/game/resource.c
    ${PROJECT_SOURCE_DIR}/src/game/settings.c
    ${PROJECT_SOURCE_DIR}/src/game/state.c
    ${PROJECT_SOURCE_DIR}/src/game/tick.c
    ${PROJECT_SOURCE_DIR}/src/game/time.c
    ${PROJECT_SOURCE_DIR}/src/game/tutorial.c
    ${PROJECT_SOURCE_DIR}/src/game/undo.c
)
set(INPUT_FILES
    ${PROJECT_SOURCE_DIR}/src/input/cursor.c
    ${PROJECT_SOURCE_DIR}/src/input/hotkey.c
    ${PROJECT_SOURCE_DIR}/src/input/keyboard.c
    ${PROJECT_SOURCE_DIR}/src/input/mouse.c
    ${PROJECT_SOURCE_DIR}/src/input/scroll.c
)
set(MAP_FILES
    ${PROJECT_SOURCE_DIR}/src/map/aqueduct.c
    ${PROJECT_SOURCE_DIR}/src/map/bookmark.c
    ${PROJECT_SOURCE_DIR}/src/map/bridge.c
    ${PROJECT_SOURCE_DIR}/src/map/building.c
    ${PROJECT_SOURCE_DIR}/src/map/building_tiles.c
    ${PROJECT_SOURCE_DIR}/src/map/desirability.c
    ${PROJECT_SOURCE_DIR}/src/map/elevation.c
    ${PROJECT_SOURCE_DIR}/src/map/figure.c
    ${PROJECT_SOURCE_DIR}/src/map/grid.c
    ${PROJECT_SOURCE_DIR}/src/map/image.c
    ${PROJECT_SOURCE_DIR}/src/map/image_context.c
    ${PROJECT_SOURCE_DIR}/src/map/natives.c
    ${PROJECT_SOURCE_DIR}/src/map/orientation.c
    ${PROJECT_SOURCE_DIR}/src/map/property.c
    ${PROJECT_SOURCE_DIR}/src/map/random.c
    ${PROJECT_SOURCE_DIR}/src/map/ring.c
    ${PROJECT_SOURCE_DIR}/src/map/road_access.c
    ${PROJECT_SOURCE_DIR}/src/map/road_aqueduct.c
    ${PROJECT_SOURCE_DIR}/src/map/road_network.c
    ${PROJECT_SOURCE_DIR}/src/map/routing.c
    ${PROJECT_SOURCE_DIR}/src/map/routing_data.c
    ${PROJECT_SOURCE_DIR}/src/map/routing_path.c
    ${PROJECT_SOURCE_DIR}/src/map/routing_terrain.c
    ${PROJECT_SOURCE_DIR}/src/map/soldier_strength.c
    ${PROJECT_SOURCE_DIR}/src/map/sprite.c
    ${PROJECT_SOURCE_DIR}/src/map/terrain.c
    ${PROJECT_SOURCE_DIR}/src/map/tiles.c
    ${PROJECT_SOURCE_DIR}/src/map/water.c
    ${PROJECT_SOURCE_DIR}/src/map/water_supply.c
)
set(SCENARIO_FILES
    ${PROJECT_SOURCE_DIR}/src/scenario/building.c
    ${PROJECT_SOURCE_DIR}/src/scenario/criteria.c
    ${PROJECT_SOURCE_DIR}/src/scenario/demand_change.c
    ${PROJECT_SOURCE_DIR}/src/scenario/distant_battle.c
    ${PROJECT_SOURCE_DIR}/src/scenario/earthquake.c
    ${PROJECT_SOURCE_DIR}/src/scenario/emperor_change.c
    ${PROJECT_SOURCE_DIR}/src/scenario/empire.c
    ${PROJECT_SOURCE_DIR}/src/scenario/gladiator_revolt.c
    ${PROJECT_SOURCE_DIR}/src/scenario/invasion.c
    ${PROJECT_SOURCE_DIR}/src/scenario/map.c
    ${PROJECT_SOURCE_DIR}/src/scenario/price_change.c
    ${PROJECT_SOURCE_DIR}/src/scenario/property.c
    ${PROJECT_SOURCE_DIR}/src/scenario/random_event.c
    ${PROJECT_SOURCE_DIR}/src/scenario/request.c
    ${PROJECT_SOURCE_DIR}/src/scenario/scenario.c
)
set(GRAPHICS_FILES
    ${PROJECT_SOURCE_DIR}/src/graphics/arrow_button.c
    ${PROJECT_SOURCE_DIR}/src/graphics/button.c
    ${PROJECT_SOURCE_DIR}/src/graphics/font.c
    ${PROJECT_SOURCE_DIR}/src/graphics/generic_button.c
    ${PROJECT_SOURCE_DIR}/src/graphics/graphics.c
    ${PROJECT_SOURCE_DIR}/src/graphics/image.c
    ${PROJECT_SOURCE_DIR}/src/graphics/image_button.c
    ${PROJECT_SOURCE_DIR}/src/graphics/lang_text.c
    ${PROJECT_SOURCE_DIR}/src/graphics/menu.c
    ${PROJECT_SOURCE_DIR}/src/graphics/panel.c
    ${PROJECT_SOURCE_DIR}/src/graphics/rich_text.c
    ${PROJECT_SOURCE_DIR}/src/graphics/screen.c
    ${PROJECT_SOURCE_DIR}/src/graphics/screenshot.c
    ${PROJECT_SOURCE_DIR}/src/graphics/text.c
    ${PROJECT_SOURCE_DIR}/src/graphics/tooltip.c
    ${PROJECT_SOURCE_DIR}/src/graphics/video.c
    ${PROJECT_SOURCE_DIR}/src/graphics/warning.c
    ${PROJECT_SOURCE_DIR}/src/graphics/window.c
)
set(SOUND_FILES
    ${PROJECT_SOURCE_DIR}/src/sound/city.c
    ${PROJECT_SOURCE_DIR}/src/sound/effect.c
    ${PROJECT_SOURCE_DIR}/src/sound/music.c
    ${PROJECT_SOURCE_DIR}/src/sound/speech.c
    ${PROJECT_SOURCE_DIR}/src/sound/system.c
)
set(WIDGET_FILES
    ${PROJECT_SOURCE_DIR}/src/widget/city.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_bridge.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_building_ghost.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_figure.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_overlay_education.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_overlay_entertainment.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_overlay_health.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_overlay_other.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_overlay_risks.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_with_overlay.c
    ${PROJECT_SOURCE_DIR}/src/widget/city_without_overlay.c
    ${PROJECT_SOURCE_DIR}/src/widget/minimap.c
    ${PROJECT_SOURCE_DIR}/src/widget/sidebar.c
    ${PROJECT_SOURCE_DIR}/src/widget/top_menu.c
)
set(WINDOW_FILES
    ${PROJECT_SOURCE_DIR}/src/window/advisors.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/chief.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/education.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/entertainment.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/financial.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/health.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/imperial.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/labor.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/military.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/population.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/ratings.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/religion.c
    ${PROJECT_SOURCE_DIR}/src/window/advisor/trade.c
    ${PROJECT_SOURCE_DIR}/src/window/build_menu.c
    ${PROJECT_SOURCE_DIR}/src/window/building_info.c
    ${PROJECT_SOURCE_DIR}/src/window/building/common.c
    ${PROJECT_SOURCE_DIR}/src/window/building/culture.c
    ${PROJECT_SOURCE_DIR}/src/window/building/distribution.c
    ${PROJECT_SOURCE_DIR}/src/window/building/figures.c
    ${PROJECT_SOURCE_DIR}/src/window/building/government.c
    ${PROJECT_SOURCE_DIR}/src/window/building/house.c
    ${PROJECT_SOURCE_DIR}/src/window/building/industry.c
    ${PROJECT_SOURCE_DIR}/src/window/building/military.c
    ${PROJECT_SOURCE_DIR}/src/window/building/terrain.c
    ${PROJECT_SOURCE_DIR}/src/window/building/utility.c
    ${PROJECT_SOURCE_DIR}/src/window/cck_selection.c
    ${PROJECT_SOURCE_DIR}/src/window/city.c
    ${PROJECT_SOURCE_DIR}/src/window/difficulty_options.c
    ${PROJECT_SOURCE_DIR}/src/window/display_options.c
    ${PROJECT_SOURCE_DIR}/src/window/donate_to_city.c
    ${PROJECT_SOURCE_DIR}/src/window/empire.c
    ${PROJECT_SOURCE_DIR}/src/window/file_dialog.c
    ${PROJECT_SOURCE_DIR}/src/window/gift_to_emperor.c
    ${PROJECT_SOURCE_DIR}/src/window/hold_festival.c
    ${PROJECT_SOURCE_DIR}/src/window/intermezzo.c
    ${PROJECT_SOURCE_DIR}/src/window/labor_priority.c
    ${PROJECT_SOURCE_DIR}/src/window/logo.c
    ${PROJECT_SOURCE_DIR}/src/window/main_menu.c
    ${PROJECT_SOURCE_DIR}/src/window/message_dialog.c
    ${PROJECT_SOURCE_DIR}/src/window/message_list.c
    ${PROJECT_SOURCE_DIR}/src/window/mission_briefing.c
    ${PROJECT_SOURCE_DIR}/src/window/mission_end.c
    ${PROJECT_SOURCE_DIR}/src/window/mission_selection.c
    ${PROJECT_SOURCE_DIR}/src/window/new_career.c
    ${PROJECT_SOURCE_DIR}/src/window/overlay_menu.c
    ${PROJECT_SOURCE_DIR}/src/window/popup_dialog.c
    ${PROJECT_SOURCE_DIR}/src/window/resource_settings.c
    ${PROJECT_SOURCE_DIR}/src/window/set_salary.c
    ${PROJECT_SOURCE_DIR}/src/window/sound_options.c
    ${PROJECT_SOURCE_DIR}/src/window/speed_options.c
    ${PROJECT_SOURCE_DIR}/src/window/trade_opened.c
    ${PROJECT_SOURCE_DIR}/src/window/trade_prices.c
    ${PROJECT_SOURCE_DIR}/src/window/victory_dialog.c
    ${PROJECT_SOURCE_DIR}/src/window/victory_video.c
)

add_executable(julius
    WIN32
    ${PLATFORM_FILES}
    ${CORE_FILES}
    ${BUILDING_FILES}
    ${CITY_FILES}
    ${EMPIRE_FILES}
    ${FIGURE_FILES}
    ${FIGURETYPE_FILES}
    ${GAME_FILES}
    ${INPUT_FILES}
    ${MAP_FILES}
    ${SCENARIO_FILES}
    ${GRAPHICS_FILES}
    ${SOUND_FILES}
    ${WIDGET_FILES}
    ${WINDOW_FILES}
    ${SMK_FILES}
    ${PROJECT_SOURCE_DIR}/res/julius.rc
)

find_package(SDL2 REQUIRED)
find_package(SDL2_mixer REQUIRED)

if(MSVC)
    add_compile_definitions(_CRT_SECURE_NO_WARNINGS)
endif()

include_directories(${SDL2_INCLUDE_DIR})
include_directories(${SDL2_MIXER_INCLUDE_DIR})

include_directories(ext)
include_directories(src)
if(MSVC)
    include_directories(ext/dirent)
endif()

target_link_libraries (julius ${SDL2_LIBRARY} ${SDL2_MIXER_LIBRARY})

install(TARGETS julius RUNTIME DESTINATION bin)

# Unit tests
enable_testing()
add_subdirectory(test)
